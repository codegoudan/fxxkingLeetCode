大家好呀，我是蛋蛋。瞎起题目的嘴强王者，乱写界的无冕之王。

提到数组，想必臭宝们都不陌生。说不定已经昂起脑瓜抖着腿，小眼神儿斜着来一句：“就那个随随便便用脚趾甲就能学会的数组？”

数组，你确定很简单？

自信点，语气肯定点，数组确实很简单。

但是我还要讲。

当然开始之前，你要确定你先看过下面这篇：

[保姆级教学！彻底学会时间复杂度和空间复杂度](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475918746&idx=1&sn=3fe42234a1f07fb084d11fe06fb24893&chksm=ff22e217c8556b019b9052f9d4805174385ba4c8c099216fa226dbd1b033a9a49782579e4b75&scene=21#wechat_redirect)

。。。

<div align=center>

![shuzu1-0](https://cdn.codegoudan.com/img/shuzu1-0.png)

</div>



---

数组，一种基础的的线性数据结构。不管写什么代码基本上都会用到数组，它也是面试中面试官喜欢提问的高频问题。

鉴于它劳模的身份，**对数组的考察基本都是考察代码方面**的问题。

但是万变不离其宗，解决数组方面问题的万能钥匙，其实就是深刻的认识数组本身的深浅。

<div align=center>

![shuzu1-1](https://cdn.codegoudan.com/img/shuzu1-1.jpg)

</div>

# 什么是数组？

首先什么是数组？

数组是一种基础的**线性数据结构**，它是用**连续的一段内存空间，来存储相同数据类型**数据的集合。

这里面有两个重点：

- 线性数据结构
- 连续内存存储相同数据类型



## 线性数据结构

线性数据结构，顾名思义，像线一样的数据结构，又硬又直的典型代表。

<div align=center>

![shuzu1-2](https://cdn.codegoudan.com/img/shuzu1-2.jpg)

</div>

**线性数据结构是有限的，它是某类元素的集合并且记录着元素之间的一组顺序关系，除了首尾之外，其余元素之间有且只有一个前驱和后继**。

除了数组以外，像单链表、栈、队列等也是典型的线性数据结构。

<div align=center>

![shuzu1-3](https://cdn.codegoudan.com/img/shuzu1-3.png)

</div>



## 连续内存存储的相同数据类型

以一个长度为 6 的 int 类型的数组为例，理解一下“连续内存存储相同数据类型”数组的样砸。

<div align=center>

![shuzu1-4](https://cdn.codegoudan.com/img/shuzu1-4.png)

</div>

上图中的计算机给数组 a 分配了一块连续的内存空间 1000 - 1005，首地址 address[0] = 1000。

因为连续，且对于数组来说下标从 0 开始的，所有对于数组中的每个元素来说，它的内存地址就很好计算：

```Python
address[i] = address[0] + i 
```



# 数组的操作

从上面可以看出，连续内存存储使得数组有一个天然的优势，这个优势就是可以**根据下标，快速的随意访问任意一个位置的数组元素。**

因为数组可以保证不管你访问哪个元素，只要给出下标，只需进行一次操作，就可以定位到元素的位置，从而拿出这个位置上的值。

这步操作的时间复杂度就是 O(1)。

当然 God 是公平的，**在查找上让数组当了快男，那必定让它在插入和删除上加上“持久”技能点。**

在这你要明白，持久意味着花的时间多（低效）。这么看来，在数据结构与算法中，秒男成了褒义词。

<div align=center>

![shuzu1-5](https://cdn.codegoudan.com/img/shuzu1-5.jpg)

</div>

这种插入和删除的“持久”，是如何产生的呢？

这还是要从“连续内存存储”上找原因，真所谓快也它，慢也它。

**连续的内存使得在插入或者删除的元素的时候，其它元素也要相应的移动。**

比如我们在下标为 2 的位置上插入一个元素 29：

<div align=center>

![shuzu1-6](https://cdn.codegoudan.com/img/shuzu1-6.png)

</div>


为了保持连续内存存储，在下标为 2 的位置上插入 29，原先 2 - 5 下标位置上元素都要向后一步走，可以看出这一步操作的时间复杂度为 O(n)。

当然在这我必须要强调一点的是，插入的时间复杂度其实根据情况的不同而不同，在这里主要因为 懒 手疼，没有都列出来，毕竟我的臭宝们是最聪明哒。

一般我都是按照**最坏情况时间复杂度**来算。对于插入来说，最好的情况肯定是插在末尾，这样所有的元素都不用动，时间复杂度为 O(1)，那最坏的情况就是在数组的开头插入，这样所有的元素都要动，时间复杂度就成了 O(n)，请大家一定要注意。

<div align=center>

![shuzu1-7](https://cdn.codegoudan.com/img/shuzu1-7.jpg)

</div>

对删除来说，和插入操作也差不多，比如我们删除下标为 2 位置上的元素。

<div align=center>
![shuzu1-8](https://cdn.codegoudan.com/img/shuzu1-8.png)

</div>

删除了下标 2 位置上的数字 a[2]， 原来下标 3 - 5 位置上的元素统统向前一步走。

同样对于删除来说，最好情况是删除末尾的元素，时间复杂度为 O(1)，最坏的情况是删除首位的元素，时间复杂度是 O(n)。

数组的查找、插入和删除这 3 个操作讲完以后，还有最后一点提醒一下大家，在做数组类问题的时候要注意**数组越界**的问题。

数组越界，简单来说就是对于一个大小为 n 的数组，它的下标应是 0 到 n-1，对这 n 个位置的元素之外的访问，就是非法的，这就叫做“越界”。

比如对于下面这两行代码：

```Python
lst = [1,2,3,4]
print(lst[4])
```

编译一下，就会提示 IndexError : list index out of range。

臭宝们平时写数组的时候一定要注意，不然一不小心就 out 了。

<div align=center>

![shuzu1-9](https://cdn.codegoudan.com/img/shuzu1-9.jpg)

</div>



