---
layout: post
title: "iOS block 工作原理"
description: "探究 oc 中 block的工作机制"
categories: [iOS]
tags: [oc, block, 内存管理]
redirect_from:
  - /2017/05/25/
---

* Kramdown table of contents
{:toc .toc}

# 什么是 block ?

Block 本质是一个 oc 对象。它有自己的构造器（系统提供），自己的函数（ block 代码），自己的析构器（系统提供）。其在 iOS 中有 3 种常见的类型，分别是NSConcreteStackBlock，NSConcreteMallocBlock，NSConcreteGlobalBlock。

# Block 如何捕获外部变量（如何将 block 中使用到的变量变成 block 对象里的实例变量）?
​
> 局部变量
> > 该类型变量被追加到 block 的构造器中，方式为传值，用于初始化 block 对象内部相应实例变量。

> 静态变量
> >该类型变量被追加到 block 的构造器中，方式为传址，用于初始化 block 对象内部相应实例变量。

​
> 静态全局变量
> >因为作为全局变量，在内存中被存放在程序的数据区，可以在程序中任何位置访问该类型的变量（无需捕获）。

> 全局变量
> >同上
​

正是因为上述局部变量的捕获方式，局部变量的值无法在 block 内部被修改。而其他 3 中类型的变量可以被修改。

# 如何在 block 内部修改局部变量？

1 传内存地址到 block 中

​即在 block 中使用指针，根据刚才所述的局部变量的捕获方式可知，指针本身将被放入 block 对象的实例变量中。指针变量本身的值不可被修改（即指针所存地址）。但我们可以修改其指向的对象。


2 在局部变量前添加 \_\_block 修饰符

带 \_\_block 修饰符的变量将被转化成一个结构体。该结构体有 5 个实例变量，分别是 isa 指针，指向该结构体类型的 forwarding 指针，标记 flags ，大小 size ，与捕获变量同名的变量。

当 block 没有被 copy 时，forwarding指针指向该结构体自身。

当 block 被 copy 时，带 \_\_block 修饰符的变量所生成的结构体也会被 copy 到堆上，此时原结构体的的 forwarding 指针不再指向原结构体自己，而是指向被拷贝到堆上的副本，然后堆上的副本的 forwarding 指针指向堆上的自己。

这样一来，无论是结构体在堆上，还是栈上，我们都能通过 struct -> forwarding -> val 访问变量。
​
# Block 的 3 种类型与是否持有被捕获的对象

1 NSConcreteStackBlock

只用到外部局部变量、成员属性变量，且没有强指针引用的block都是StackBlock。StackBlock的生命周期由系统控制的，一旦返回之后，就被系统销毁了。

不持有捕获的对象。
​

2 NSConcreteMallocBlock

​
有强指针引用或copy修饰的成员属性引用的block会被复制一份到堆中成为MallocBlock，没有强指针引用即销毁，生命周期由程序员控制。
​

持有捕获的对象。
​

3 NSConcreteGlobalBlock

​
没有用到外部变量或只用到全局变量、静态变量的block为_NSConcreteGlobalBlock，生命周期从创建到应用程序结束。

不持有捕获的对象。
​

注：在 ARC 环境下，当我们将 block 赋值给变量时，就会触发 block 的 copy 操作。block的类型变为 MallocBlock 。


[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture