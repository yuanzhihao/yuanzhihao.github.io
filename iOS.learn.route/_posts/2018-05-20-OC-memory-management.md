---
layout: post
title: "OC 内存管理"
description: "OC 的内存管理"
categories: [iOS]
tags: [oc, 内存管理]
redirect_from:
  - /2018/05/25/
---

* Kramdown table of contents
{:toc .toc}

# 引用计数

## 什么是引用计数？

首先，我们要明白什么是内存管理。内存管理是为了能够快速分配内存，并且适当的时候释放并且回收内存。而引用计数就是为了达成上述目标的一种策略。在 OC 中，当我们创建一个新对象时，该对像引用计数为1；有一个新的指针关联到该对象时，他的引用计数就加1；对象关联的某个指针不再指向他时，他的引用计数就减1；对象的引用计数为0时，说明此对象不再被任何指针指向，这时我们就可以将对象销毁。

引用计数式内存管理思维：

|---
| 思维 | OC 对应的方法 | 对 OC 对象做的动作 | 对对象引用计数的影响 |
|-|:-|:-:|-:
| 自己生成对象，自己所持有 | alloc/new/copy/mutableCopy等方法 | 生成对象 | 引用计数为1 |
| 非自己生成的对象，自己也能持有 | retain方法 | 持有对象 | 引用计数+1 |
| 不再需要自己持有的对象时释放 | release方法 | 释放对象或废弃对象（引用计数归零）| 引用计数-1 |
| 非自己持有的对象无法释放 |

## 引用计数的存储及维护

在 OC 中，我们采用引用计数管理内存，那么我们又如何存储和维护引用计数呢？答案是 SideTable，OC 使用这种数据结构来存储引用计数，其结构如下：

```C++
struct SideTable {
    // 保证原子操作的自旋锁
    spinlock_t slock;
    // 引用计数的 hash 表
    RefcountMap refcnts;
    // 弱引用全局 hash 表
    weak_table_t weak_table;

    // 构造函数，将 sidetable 初始化为 0x0
    SideTable() {
        memset(&weak_table, 0, sizeof(weak_table));
    }

    // 析构函数，不应该被调用
    ~SideTable() {
        _objc_fatal("Do not delete SideTable.");
    }

    // 锁操作
    void lock() { slock.lock(); }
    void unlock() { slock.unlock(); }
    void forceReset() { slock.forceReset(); }

    // Address-ordered lock discipline for a pair of side tables.

    template<HaveOld, HaveNew>
    static void lockTwo(SideTable *lock1, SideTable *lock2);
    template<HaveOld, HaveNew>
    static void unlockTwo(SideTable *lock1, SideTable *lock2);
}
```

在 OC 的 Runtime 中，存在全局的 SideTables，其本质是一张哈希表，它的 key 是对象的地址，它的 value 是该对象的 SideTable，即引用计数。

但是，为什么结构体中的 refcnts 是一张 hash 表而不是一个整数呢？是苹果弄错了吗？

当然不是，其实这么设计是为了平衡锁的成本和读取效率。因为采用对象的地址作为 key，我们完全可以避免 SideTables 的 Hash 冲突。这样一来，SideTable 就不需要 hash 表存储引用计数，而只需要一个整数。但这样也带来了一个问题，就是每个 SideTable 都会带着一把锁，保证操作的原子性，避免 race condition。如此一来，系统就需要维护大量的锁，由此带来了极大的开销。同时，也是难以维护的。所以，苹果有意的制造了 hash 冲突，将不同的对象指向同一个 SideTable。再通过 refcnts 这张引用计数 hash 表，获取每个对象引用计数。

其实这个过程就相当于为一批学生安排住宿。我们有一栋宿舍楼（SideTables）里面有很多个房间（SideTable），每个房间都会有把锁（slock）。每个房间会住上多个学生（引用计数），每个房间的床位分配表就是 refcnts。

但其实 refcnts 的 value，其本身的值并不是引用计数。它的类型是 size_t（无符号整数），被划分成分了 4 个区域，最高位是溢出标志位，最低位是是否有弱引用的标志位，第二位是是否在 dealloc 的标志位。剩下的部分才用于存储真正的引用计数，所以每次我们引用计数加一时，真正加的是4，在取出真正的引用计数时需要右移两位。

![refcnts 的 value](./refcnts的value.png)

### 为什么使用自旋锁保证数据安全？

自旋锁比较适用于锁使用者保持锁时间比较短的情况。正是由于自旋锁使用者一般保持锁时间非常短，因此选择自旋而不是睡眠是非常必要的，自旋锁的效率远高于互斥锁。信号量和读写信号量适合于保持时间较长的情况，它们会导致调用者睡眠，因此只能在进程上下文使用，而自旋锁适合于保持时间非常短的情况，它可以在任何上下文使用。



## 弱引用表

在结构体 SideTable 中，存在一个弱引用全局 hash 表，类型为 weak_table_t，结构如下：
​
```C
struct weak_table_t {
    weak_entry_t *weak_entries;
    size_t    num_entries;
    uintptr_t mask;
    uintptr_t max_hash_displacement;
};
```

weak_entries 是存放弱引用的数组。num_entries 是存放的 weak_entry_t 条目的数量。mask 是动态申请的弱引用数组 weak_entries 长度减 1 的值，用来对哈希后的值取余和记录数组大小。max_hash_displacement 是哈希碰撞后最大的位移值（因为 weak_table_t 使用了开放寻址法来解决 hash 冲突）。

其实 weak_table_t 就是一个动态增长的哈希表。当 weak_table 里的弱引用条目达到它容量的四分之三时，便会将容量拓展为两倍。缩小的话则是需要表本身大于等于 1024 并且存放了不足十六分之一的条目时，直接缩小 8 倍。以上工作都是交给了 weak_resize 函数完成。其实质就是新建一个数组，将老数组里的值使用 weak_entry_insert 函数添加进去。注意到代码中间 mask 在这里被赋值为新数组的大小减去 1，max_hash_displacement 和 num_entries也都清零了，因为 weak_entry_insert 函数会对这两个值进行操作。

mask 用于和哈希后的对象的地址进行 & 操作，因为弱引用表的大小永远是 2 的幂，mask 则是大小减去 1 即为一个 0b111...11 这么一个数，和它进行 & 运算相当于取余。

weak_entry_t 的结构：

```C++
#define WEAK_INLINE_COUNT 4

#define REFERRERS_OUT_OF_LINE 2

struct weak_entry_t {
    DisguisedPtr<objc_object> referent;
    union {
        struct {
            weak_referrer_t *referrers;
            uintptr_t        out_of_line_ness : 2;
            uintptr_t        num_refs : PTR_MINUS_2;
            uintptr_t        mask;
            uintptr_t        max_hash_displacement;
        };
        struct {
            // out_of_line_ness field is low bits of inline_referrers[1]
            weak_referrer_t  inline_referrers[WEAK_INLINE_COUNT];
        };
    };

    bool out_of_line() {
        return (out_of_line_ness == REFERRERS_OUT_OF_LINE);
    }

    weak_entry_t& operator=(const weak_entry_t& other) {
        memcpy(this, &other, sizeof(other));
        return *this;
    }

    weak_entry_t(objc_object *newReferent, objc_object **newReferrer)
        : referent(newReferent)
    {
        inline_referrers[0] = newReferrer;
        for (int i = 1; i < WEAK_INLINE_COUNT; i++) {
            inline_referrers[i] = nil;
        }
    }
};
```

首先 DisguisedPtr\<T\> 类型和 T* 的行为是一模一样的，这个类型存在的目的是为了躲过内存泄漏工具的检查。

referent 这个指针记录的便是被弱引用的对象。接下来的联合里有两种结构体，先分析第一种：

* referrers：referrers 是一个 weak_referrer_t 类型的数组，用来存放弱引用变量的地址，weak_referrer_t 的定义是这样的：typedef DisguisedPtr\<objc_object *\> weak_referrer_t;
* out_of_line_ness：2 bit 标记位，用来确定联合里的内存是第一个结构体还是第二个结构体;
* num_refs：PTR_MINUS_2 便是字长减去 2 位，和 out_of_line_ness 一起组成一个字长，用来存储 referrers 的大小;
* mask 和 max_hash_displacement：和前面分析的一样，做哈希表用到的东西;

可以发现第一种结构体也是一个哈希表，第二种结构体则是一个和第一种结构体一样大的数组，所谓的 inline 存储。存放思路则是首先 inline 存储，当超过 WEAK_INLINE_COUNT 也就是 4 时，再变成第一种的动态哈希表存储。代码下方的构造函数便体现了这个思路。

可以注意到 weak_entry_t 重载了赋值操作符，将赋值变成了一个拷贝内存的操作。


[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture