---
layout: post
title: "Toll-Free Bridged Types 桥接类型"
description: "Toll-Free Bridged Types 桥接类型"
categories: [iOS]
tags: [Toll-Free Bridged]
redirect_from:
  - /2018/07/14/
---

* Kramdown table of contents
{:toc .toc}

# 什么是桥接类型？

桥接类型是指可以在 Core Foundation 框架和 Foundation 框架中可交换使用的一对类型。这意味着你可以使用同一种数据结构作为 Core Foundation 函数调用的参数或是 OC 消息的接收者。表-1列出了支持桥接的类型，

| Core Foundation type | Foundation class | Availability
| CFArrayRef | NSArray | OS X 10.0
| CFAttributedStringRef | NSAttributedString | OS X 10.4
| CFBooleanRef | NSNumber | OS X 10.0
| CFCalendarRef | NSCalendar | OS X 10.4
| CFCharacterSetRef | NSCharacterSet | OS X 10.0
| CFDataRef | NSData | OS X 10.0
| CFDateRef | NSDate | OS X 10.0
| CFDictionaryRef | NSDictionary | OS X 10.0
| CFErrorRef | NSError | OS X 10.5
| CFLocaleRef | NSLocale | OS X 10.4
| CFMutableArrayRef | NSMutableArray | OS X 10.0
| CFMutableAttributedStringRef | NSMutableAttributedString | OS X 10.4
| CFMutableCharacterSetRef | NSMutableCharacterSet | OS X 10.0
| CFMutableDataRef | NSMutableData | OS X 10.0
| CFMutableDictionaryRef | NSMutableDictionary | OS X 10.0
| CFMutableSetRef | NSMutableSet | OS X 10.0
| CFMutableStringRef | NSMutableString | OS X 10.0
| CFNullRef | NSNull | OS X 10.2
| CFNumberRef | NSNumber | OS X 10.0
| CFReadStreamRef | NSInputStream | OS X 10.0
| CFRunLoopTimerRef | NSTimer | OS X 10.0
| CFSetRef | NSSet | OS X 10.0
| CFStringRef | NSString | OS X 10.0
| CFTimeZoneRef | NSTimeZone | OS X 10.0
| CFURLRef | NSURL | OS X 10.0
| CFWriteStreamRef | NSOutputStream | OS X 10.0


## 类型转换和对象生命周期管理

虽然有桥接机制，但在使用时仍需要进行类型转换使编译器正常工作。除此之外，可能还需要指出对象生命周期管理方式。

编译器不会自动管理 Core Foundation 框架中的对象的生命周期。使用者需要通过使用类型转换或者 Core Foundation-style 宏来告诉编译器对象生命周期的管理方式。

* \_\_bridge 只转换类型，不对对象生命周期的管理类型做改变
* \_\_bridge_retained 或 CFBridgingRetain 将 OC 类型的指针转换为 Core Foundation 指针，同时也将对象的生命周期管理方式转换为 malloc/release 方式。使用者需要在不再需要该对象时调用 CFRelease 函数释放对象。
* \_\_bridge_transfer 或 CFBridgingRelease 将非 OC 指针转换为 OC 指针，同时将对象的生命周期管理方式转换为 ARC。

例子，

```Objective-C
NSMutableArray *colors = [NSMutableArray arrayWithObject:(id)[[UIColor darkGrayColor] CGColor]];
[colors addObject:(id)[[UIColor lightGrayColor] CGColor]];
```

```Objective-C
NSLocale *gbNSLocale = [[NSLocale alloc] initWithLocaleIdentifier:@"en_GB"];
CFLocaleRef gbCFLocale = (__bridge CFLocaleRef)gbNSLocale;
CFStringRef cfIdentifier = CFLocaleGetIdentifier(gbCFLocale);
NSLog(@"cfIdentifier: %@", (__bridge NSString *)cfIdentifier);
// Logs: "cfIdentifier: en_GB"
 
CFLocaleRef myCFLocale = CFLocaleCopyCurrent();
NSLocale *myNSLocale = (NSLocale *)CFBridgingRelease(myCFLocale);
NSString *nsIdentifier = [myNSLocale localeIdentifier];
CFShow((CFStringRef)[@"nsIdentifier: " stringByAppendingString:nsIdentifier]);
// Logs identifier for current locale
```

```Objective-C
- (void)drawRect:(CGRect)rect {
 
    CGContextRef ctx = UIGraphicsGetCurrentContext();
    CGColorSpaceRef colorSpace = CGColorSpaceCreateDeviceGray();
    CGFloat locations[2] = {0.0, 1.0};
    NSMutableArray *colors = [NSMutableArray arrayWithObject:(id)[[UIColor darkGrayColor] CGColor]];
    [colors addObject:(id)[[UIColor lightGrayColor] CGColor]];
    CGGradientRef gradient = CGGradientCreateWithColors(colorSpace, (__bridge CFArrayRef)colors, locations);
    CGColorSpaceRelease(colorSpace);  // Release owned Core Foundation object.
 
    CGPoint startPoint = CGPointMake(0.0, 0.0);
    CGPoint endPoint = CGPointMake(CGRectGetMaxX(self.bounds), CGRectGetMaxY(self.bounds));
    CGContextDrawLinearGradient(ctx, gradient, startPoint, endPoint,
                                kCGGradientDrawsBeforeStartLocation | kCGGradientDrawsAfterEndLocation);
    CGGradientRelease(gradient);  // Release owned Core Foundation object.
}
```

## 桥接的内部原理

### 从 Core Foundation 对象到 Foundation 对象的桥接

这个方向的桥接是十分直接的。事实上，每个 OC 中的桥接类型都是一个类簇，这意味着公开的类实际上是一个抽象类，真正的功能是由一个私有的子类实现的。Core Foundation 中的类和这些私有子类之一具有相同的内存布局。其它的 OC 私有子类可能与之不同，但从外部看，它们都是一样的，因为它们具有相同的接口。

具体来说，以 NSString 为例，NSString 是一个抽象类。每次创建 NSString 对象，实际上都是得到一个它的子类的实例。

NSCFString 是这些私有子类的其中之一。它和 CFString 是对等的。可以被视作一个 CFString 对象的 isa 指针指向 NSCFString 类类型。这使得它能像一个 OC 对象一样工作。



### [receiver setValue:(nullable id)value forKeyPath:(nonnull NSString \*)keyPath]

在调用上述方法时，可以看做是将 keyPath 分割成一个个 key，再对除最后一个 key 以外的所有 key 链式调用 [receiver valueForKey:(nonnull NSString \*)key。对最后一个 key 调用[receiver setValue:(nullable id)value forKeyPath:(nonnull NSString \*)keyPath]。且在以上步骤中，任何一个 keyPath 上的对象如果不兼容 KVC，都会调用 [receiver setValue:(nullable id)value forUndeifinedKey:(nonnull NSString \*)key]。

### [receiver setValuesForKeysWithDictionary:(nonnull NSArray<NSString \*> \*)keys]

在调用上述方法时，可以看做对字典中的每对 key 和 value，单独调用 [receiver setValue:(nullable id)value forKey:(nonnull NSString \*)key]。如果某个 key 对应属性的值为 NSNull，就自动将其转化为 nil。（因为 OC 对象中的空用 nil 来表示）

### [receiver setNilValueForKey:(NSString \*)key] 

在 [receiver setValue:(nullable id)value forKey:(nonnull NSString \*)key] 的默认实现中，如果将 nil 赋值给非对象变量，KVC 将调用 [receiver setNilValueForKey:(NSString \*)key]。该方法的默认实现将抛出异常。

## 通过 KVC 获取属性的值 

### [receiver valueForKey:(nonnull NSString \*)key] 

在调用上述方法时，会优先按序调用 receiver 中 key 对应的 getter 方法，即 get<Key>，<key>，is<Key>，\_<key>。如果这些 setter 方法都没有找到，则会检查 receiver 中是否存在 countOf<Key>，objectIn<Key>AtIndex: 和 <key>AtIndexes:。如果同时找到了第一个方法和后两者中的一个， KVC 就会将被访问的属性看作数组类型。KVC 将创建一个数组代理对象，用来响应所有的 NSArray 方法，并将这个代理对象返回，虽然它实际上并不是数组对象。之后数组代理对象所接收到的消息将会被转化成 countOf<Key>, objectIn<Key>AtIndex: 和 <key>AtIndexes: 消息的组合发送给创建这个数组代理对象的对象，在此即 receiver。此外如果 receiver 还实现了可选的方法，例如，get<Key>:range:，那么在消息的转化过程中，这些方法也会根据情况被使用。如果没有将被访问的属性看做数组类型（即不满足上一个条件），就会检查 receiver 中是否存在 countOf<Key>，enumeratorOf<Key> 和 memberOf<Key>:。如果这些方法全部存在，那么 KVC 就会将访问的属性看作 Set 类型。 KVC 将创建一个集合代理对象，用来响应所有的 NSSet 方法。同样的，这个代理对象会将接收到的消息转化成 countOf<Key>, enumeratorOf<Key> 和 memberOf<Key>: 消息的组合发送给创建这个集合代理对象的对象，在此即 receiver。如果也没有将被访问的属性看做 Set 类型，KVC 会调用 receiver 的 + (BOOL)accessInstanceVariablesDirectly 方法。如果返回 YES，将按序查找以下实例变量：\_<key>, \_is<Key>, <key> 和 is<Key>。如果找到了相应的实例变量且是对象类型就直接返回该值。如果是值类型且支持 NSNumber，将其包装成 NSNumber 并返回。如果不支持 NSNumber，就将其包装成 NSValue 并返回。如果没有找到以上任意一个实例变量，就调用 [receiver valueForUndeifinedKey:(nonnull NSString \*)key]。该方法的默认实现将抛出异常。

### [receiver valueForKeyPath:(nonnull NSString \*)keyPath] 

在调用上述方法时，可以看做是将 keyPath 分割成一个个 key，再链式调用 [receiver valueForKey:(nonnull NSString \*)key]，且任何一个 keyPath 上的对象如果不兼容 KVC，都会调用 [receiver valueForUndeifinedKey:(nonnull NSString \*)key]。

### [receiver dictionaryWithValuesForKeys:(nonnull NSArray<NSString \*> \*)keys]

在调用上述方法时，可以看做对数组中的每个 key，单独调用 [receiver valueForKey:(nonnull NSString \*)key]，再将 key 和 上述方法返回的结果存入字典并返回该字典。如果某个 key 对应属性的值为 nil，就自动将其转化为 NSNull。（因为 collection 类型的对象中不允许存入 nil 作为值）

## 通过 KVC 访问集合类型

一般来说，通过 KVC 访问集合类型和访问一般类型没有什么不同（见上文）。但当你想修改集合类型的值时，更有效的方法是使用 NSKeyValueCodingKVC 协议提供的获取可变集合类型的方法。

* mutableArrayValueForKey: and mutableArrayValueForKeyPath:
* mutableSetValueForKey: and mutableSetValueForKeyPath:
* mutableOrderedSetValueForKey: and mutableOrderedSetValueForKeyPath:

其实这些方法和上文中 valueForKey: 访问集合类型一样，本质上返回的都是代理对象而非集合对象本身。只是这些代理对象通过对其它各种方法的调用，可以让我们在使用时将其视作相应的集合对象。并且在许多情况下，这种方式比直接使用可变属性更有效。此外，这种方式能为集合内元素的 KVO 兼容提供额外的收益。

TODO: 补充这种方式能为集合内元素的 KVO 兼容提供的额外收益。

## 在 valueForKeyPath: 中嵌入集合操作符

[!keyPath][https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueCoding/art/keypath.jpg]

左 keyPath 代表要访问的集合。右 keyPath 代表被集合操作符操作的集合中元素的属性。

集合操作符有三类，

* 聚合操作符
 * @avg                            求集合中所有元素右 keyPath 的平均值，nil 将被视作 0。
 * @count                          统计集合元素的个数，可以无右 keyPath，若有无视之。
 * @max                            找出集合中所有元素右 keyPath 的最大值，使用元素的 compare: 方法进行比较。nil 将被无视。
 * @min                            找出集合中所有元素右 keyPath 的最小值，使用元素的 compare: 方法进行比较。nil 将被无视。
 * @sum                            求集合中所有元素右 keyPath 的和，nil 将被视作 0。
* 数组操作符
 * @distinctUnionOfObjects         返回一个由集合中不相同右 keyPath 组成的数组（即每个值只会在返回的数组中出现一次，无论其在原集合中出现多少次）
 * @unionOfObjects                 返回一个由集合中所有右 keyPath 组成的数组（即每个值会在出现和在原集合中相同次数）
* 嵌套操作符
 * @distinctUnionOfArrays          返回一个由两层嵌套集合中不相同右 keyPath 组成的数组（即每个值只会在返回的数组中出现一次，无论其在原集合中出现多少次）
 * @unionOfArrays                  返回一个由两层嵌套集合中所有右 keyPath 组成的数组（即每个值会在出现和在原集合中相同次数）
 * @distinctUnionOfSets            返回一个由两层嵌套集合中不相同右 keyPath 组成的 set（即每个值只会在返回的数组中出现一次，无论其在原集合中出现多少次）

注：数组操作符和嵌套操作符操作的右 keyPath 不允许出现 nil，否则会抛出异常。

# KVO

KVO 是一套通知对象其他对象的特定属性发生改变的机制。它在 model 和 controller 的通信中十分有用。在被通知改变发生时，我们可以获取到改变的类型和被改变的对象。

要使用 KVO，首先要确认被观察的对象是 KVO 兼容的。如果对象是继承自 NSObject 且被观察的属性是通过常规方法创建的，那么这个对象和属性就自动是 KVO 兼容的了。除此以外，我们也可以手动实现 KVO 兼容（将会在后面介绍）。

确认 KVO 兼容后，我们需要被观察的对象中注册观察者，通过调用被观察者的 addObserver:forKeyPath:options:context: 方法来实现这一目的。然后需要在观察者中实现 observeValueForKeyPath:ofObject:change:context: 用于接受发生改变的通知。

当观察者将被销毁时，一定要把被观察者的自己注册观察者信息移除，通过调用被观察者的 removeObserver:forKeyPath: 来实现这一目的。

## 手动 KVO 兼容

要实现手动 KVO 兼容，首先一定要确保该类型是遵循 KVC 机制的，

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture