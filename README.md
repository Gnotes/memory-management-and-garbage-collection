# JS 中的内存管理 与 垃圾回收

> 通过对参考文档的阅读，整理了如下文字

**1.** 在 `JavaScript` 中申明变量时就会自行内存分配，并不在使用时进行自动的释放，“释放过程” 叫做垃圾回收。

**2.** 不管什么程序语言，**内存生命周期** 基本是一致的

> ① 分配->你所需要的内存  
> ② 使用->分配到的内存（读、写）  
> ③ 释放->不需要的变量

**3.** 内存空间主要分为两部分：`堆`(heap)、`栈`(stack)

堆和栈都是 **运行时** 内存中分配的一个数据区，因此也被称为堆区和栈区

> 堆（heap）: 用于**复杂数据类型**（引用类型）分配空间，例如数组对象、object 对象；它是运行时动态分配内存的，因此存取速度较慢

> 栈（stack）: 中主要存放一些**基本类型的变量和对象的引用地址**,其优势是存取速度比堆要快，并且栈内的数据可以共享，但缺点是存在栈中的数据大小与生存期必须是确定的，缺乏灵活性，遵循后进先出的原则

> 池 (pool) : 存放常量，即基本数据类型的值，一般也归为“栈”中

> 基础数据类型： Number String Null Undefined Boolean

示例:

```js
var a = 1;
var d = new Number(1);
var b = { c: "i am a string" };
```

```
  栈                               堆                       池
[ b : 堆地址(0x***) ] -> [ { c: "i am a string" } ]        [   ]
[ d : 堆地址(0x***) ] -> [     new Number(1)      ]   -->  [ 1 ] // 指向同一个 1
[ a : 池地址(0x***) ] -> [                        ]   -->  [   ]
```

**4.** 垃圾回收机制

_4.1_ 引用计数 (新版浏览器中已淘汰)

> 一个 **对象** 如果有访问另一个对象的权限（隐式或者显式），叫做一个对象引用另一个对象  
> 注: “对象”的概念不仅特指 JavaScript 对象，还包括函数作用域或者全局 "**词法**" 作用域

_4.2_ 标记清除

> 这个算法假定设置一个叫做 **根**（root）的对象（在 Javascript 里，根是全局对象），从根开始，垃圾回收器将找到所有可以获得的对象和**收集标记所有不能获得的对象**

_4.3_ Chrome V8 引擎回收机制：分代回收

> 算法基于 `Generational Collection`，内存被划分为两种，分别称为 `Young Generation`（YG）和 `Old Generation`（OG）

> `YG` 又被 **平分** 为两部分空间，分别称为 `From` 和 `To`，通过对 To 区域的内存分配，并交换 From <-> To 区域，进行垃圾回收

_4.4_ 其他垃圾回收算法:

`标记-压缩`、 `GC复制算法`、 `保守式GC`、 `增量式GC`

> 查看：[几种垃圾回收算法](https://www.jianshu.com/p/a8a04fd00c3c)

## 参考

- [Mozilla 内存管理](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Memory_Management)
- [JavaScript 内存机制](https://www.cnblogs.com/liangyin/p/7764232.html)
- [JavaScript 内存泄漏教程](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)
- [JS 内存管理机制](https://segmentfault.com/a/1190000021320686?utm_source=tag-newest)
- [JS 内存管理](https://segmentfault.com/a/1190000013304880)
- [谈谈 JS 垃圾回收机制](https://segmentfault.com/a/1190000018605776?utm_source=tag-newest)
- [几种垃圾回收算法](https://www.jianshu.com/p/a8a04fd00c3c)
- [V8 之旅：垃圾回收器](http://newhtml.net/v8-garbage-collection/)
- [JS 中的垃圾回收与内存泄漏](https://segmentfault.com/a/1190000012738358)
- [深入理解 JS 垃圾回收](https://www.jb51.net/article/162416.htm)
- [聊聊 V8 引擎的垃圾回收](https://segmentfault.com/a/1190000014383214)
