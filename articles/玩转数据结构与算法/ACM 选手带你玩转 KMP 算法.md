大家好伐，我是侬们的帅蛋。

今天来学能让小儿止啼的 KMP，是不是很慌？

本来在本蛋的计划里，KMP 的顺序还是要往后放一下，但是架不住小婊贝催问。

<div align=center>

![701f8e7b90688a0fb11affe4be84b88](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_103440152_0.jpg)

</div>

那还说啥，直接安排！就今天，直奔**字符串模式匹配！**

**你们的需求就是我肝的方向！**上面的小老弟留言一次哪成，看着不像铁蛋粉。

这次保证安排的明明白白的，文章除了理解 KMP 算法的本质，拿去应付考试题也保证妥妥的！

<div align=center>

![e38a05e9f20deb93bea24eca8f34380](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_103531018_0.jpg)

</div>



---

**字符串模式匹配**是字符串中最重要的操作之一，**就是找模式串在主串中的位置**。



>**臭宝**：呃，啥是**主串**，啥是**模式串**咧？
>
>**蛋蛋**：如果你要找字符串 B 在字符串 A 中的位置，那么字符串 A 就是主串，字符串 B 就是模式串。可以理解主串就是大串，模式串就是小串。



字符串模式匹配算法有很多，但是要说提起字符串匹配，大家脑阔里的第一反应还是看毛片，呃，KMP 算法。

KMP 算法匹配字符串非常高效，但是原理有些复杂，学起来脑瓜子嗡嗡的，是学生时代学数据结构与算法绕不过去的坎儿。

不过不慌，这玩意本帅蛋门儿清。掏出你的卫生纸，呃，草稿纸，整活。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_103626347_0.jpg" alt="0cca0f691dbe9a91e0b42544c50a88e" style="zoom:67%;" />

</div>



---

要学 KMP 模式匹配算法，首先先明白之前的匹配有多笨，所以我们先来看无脑流暴力匹配。



# 暴力模式匹配

暴力模式匹配，无脑流的典型代表，没办法情况下的无冕之王。

假设主串的长度为 n，模式串的长度为 m，暴力模式匹配就是对主串的下标为 0 1 2 3 ... n - m 的元素作为模式串的开头，与模式串依次进行匹配，看有没有与模式串完全匹配的。

下面来看一个例子。

<div align=center>

![6553058b532269600a8f53cc7d88b0e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_103735184_0.jpg)

</div>

从上图可以看出，暴力匹配是在主串的长度下做模式串次匹配。

最好的情况就是从第 1 次开始就匹配成功，此时的时间复杂度就是 O(1)。

<div align=center>

![91c4991bab43774e741cc39c00a2fa4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_103755204_0.jpg)

</div>

最坏情况的情况是，模式串每次都匹配到最后一个才发现和主串不同。比如主串是“aaaaab”，模式串是“aab”。

<div align=center>

![f64389bc4547760b9dd13bce79b89eb](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_103813893_0.jpg)

</div>

看上图，除了最后一次，其余的都是每次匹配到最后，才发现，啊，我们不一样。

这种情况下，上图中，模式串在前 3 次，每次都要匹配 3 次，并且不匹配，直到第 4 次，全部匹配，不需要继续移动，所以匹配的次数为（6 - 3 + 1）* 3 = 12 次。

由此可知，对于主串长度为 n，模式串长度为 m ，最坏情况下的时间复杂度为 O((n - m + 1) * m) = O(n * m)。

你看，暴力模式匹配这么低效，你能忍的了？

忍不了吧，来，看毛片！

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_103834481_0.jpg" alt="d11ef50252d54448fa4f550d9ec72c8" style="zoom:67%;" />

</div>



# KMP 模式匹配

**KMP 算法全称Knuth Morris Pratt**，是由 D.E.Knuth、J.H.Morris 和 V.R.Pratt 三位大佬一起捣鼓出来的。

**KMP 算法目的是为了快速的从主串中找到模式串**，强调的是快，那咋快的呢？

那肯定是去掉暴力模式匹配中的”无脑“的部分。

<div align=center>

![0bd25db07add8d8fad264d03bbb5c1d](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104116493_0.jpg)

</div>

上图是暴力模式匹配的前 2 步，有比较指针 i 和 j。

可以看到，在第 1 次匹配的时候，比较指针 i 和 j 走到了下标为 5 的位置发现不匹配的元素，然后进行第 2 次匹配，此时 i 回到了下标为 1 的位置。

如果继续匹配下去的话，你会发现比较指针 i 在频繁的来回折返跑，就像上图第 1 次匹配的时候 i 已经走到了 5，等到了第 2 次匹配的时候 i 又回到了 1。

这种动作叫做**回溯**，而造成暴力模式匹配效率低下的主要坏蛋就是频繁的比较指针回溯。

**KMP 算法做到的就是比较指针不回溯，仅仅是后移模式串。**

我们接下来看，这具体是怎么实现的。下面这部分要慢一点，好好体会。

还是以主串 S = abcabcabda，模式串 T = abcabd 为例。

<div align=center>

![86539c49b0307f4af82852d861944b5](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104153197_0.jpg)

</div>

上图是模式串与主串的第一次匹配，在 i = 5，j = 5 的时候出现不匹配的元素，在此之前的元素都是匹配的。

<div align=center>

![9963bb38e890a4ce9137dc311cfbb57](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104217070_0.jpg)

</div>

观察一下模式串，你会发现在匹配的部分里，存在相同前后缀的部分。

那第二步的匹配就可以像下图这样做，这也是 KMP 算法的核心。

<div align=center>

![d50598d9de2ca579faf7780a353d1d8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104246045_0.jpg)

</div>

看懂了么？前缀直接滑到了后缀的位置上。

为什么可以这么整呢？

因为在第 1 次的时候已经比较过了，在与主串完全匹配的部分里，模式串第 1 个和第 2 个元素与第 3 个、第 4 个完全一样。

此时主串的比较指针 i 继续从 i = 5 开始比较，模式串的比较指针 j 也不需要从 0 开始直接比较，可以从 j = 2 的位置开始比较。

<div align=center>

![60266587bae6717dcf3e2beb754d9fc](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104311702_0.jpg)

</div>

你看去掉了很多无意义的匹配和回溯，大大提高了效率。

那么现在的问题成了每次不匹配的时候，找之前已匹配部分中，模式串的前后缀，而且还是**最长公共前后缀**。

也就是碰到某个不匹配的时候，我这个模式串要从最长前缀后滑动到最长后缀的位置（其实就是比较指针 j 从最长前缀移动到最长后缀的位置）。而保存这个位置的数组就是 next 数组。



## 求 next 数值

next 数组的求解，一向是老大难问题，而且这玩意不只是在写代码的时候折磨人，大学的考试或者考研考试只要涉及数据结构与算法这门课，简直是必考题。

本来想写一下我之前一直常用的求 next 的方法，后来偶然看了 b 站 up 主**正月点灯笼**的求法，感觉好像更容易理解一些，再此借鉴过来分享给大家。

在这以模式串 T = ababc 为例。



>学校用的书呢，讲 KMP 的时候，大多数数组是从下标为 1，而不是 0 开始，所以为了更方便的讲解，这个例子的讲解我会默认数组是从下标为 1 开始的（如果你习惯从 0 开始，就是数组从 1 开始的结果每个值 -1 即可，下面会讲到）。



（1）写出模式串 T 的各个前缀

<div align=center>

![5d9033d27152a4997659e8606a6eed7](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104435751_0.jpg)

</div>

（2）对于模式串 T 所有的前缀，找每一个前缀的最长公共前后缀。而且最长公共前后缀要比原始字符要短（如果一样长的话则没有意义，因为我们要的是滑动）。

在这里以 “abab” 这个串为例，这时比较指针指向最后一位的时候，出现不匹配：

<div align=center>

![2dac10349fdd49cf436a2ebb42ae2b8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104500663_0.jpg)

</div>

那么对于前 3 位 “aba” 来说，首先找长度为 2 的公共前后缀，最长前缀是 “ab”，最长后缀是 “ba”，显然前缀 ≠ 后缀；再来看长度为 1 的公共前后缀，最长前缀是 “a”，最长后缀也是 “a”，最长前缀 = 最长后缀，所以 “abab” 这个最长公共前后缀的长度 = 1。

正常来说，模式串的滑动就是从最长公共前缀 a 滑倒了最长公共后缀 a 的位置，比较指针 j 从 2 开始重新比较。

<div align=center>

![2a0f709bad51c2935989cb906a9f89d](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104526186_0.jpg)

</div>

一个看不出啥规律，再来看 “ababc”，同样比较指针指向最后一位的时候，出现不匹配：

<div align=center>

![43f336f6f89b999b5da6b88abbd2fd0](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104608549_0.jpg)

</div>

对于前 4 位 "abab"　首先找长度为 3 的公共前后缀 ，最长前缀是 “aba”，最长后缀是 “bab”，二者不相等；再找长度为 2 的，最长前缀是 “ab”，最长后缀是 “ab”，二者相等，所以 “ababc” 这个串的最长公共前后缀的长度 = 2。

所以，模式串的滑动从最长前缀 ab 滑到了最长后缀 ab，比较指针 j 从下标为 3 的位置开始重新比较。

<div align=center>

![55dd127c8a4d0bd441aea8c3cee3cb0](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104635891_0.jpg)

</div>

通过上面这两个图不知道你发现了规律没有：

**比较指针 j 的所处的位置 = 最长公共前后缀的长度 + 1。**

所以最后模式串 T = ababc 的 next 数组的值为下图：

<div align=center>

![8cd25bb4bd43119c54a4e4cc4b86283](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104656662_0.jpg)

</div>

至于第 1 个为什么是 0，你可以理解为当第 1 个不匹配的话，它的前面是没有任何元素的，记为 -1，那么他这里的值就是 -1 + 1 = 0。

所以对整个模式串 T 求 next 值就齐活了。

<div align=center>

![b3979ac3d25adc9484831dd854d841c](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104726955_0.jpg)

</div>

如果你习惯从 0 开始，就是数组从 1 开始的结果每个值 -1：

<div align=center>

![460af406232da96be5071977c12f232](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104745406_0.jpg)

</div>

```Python
# 代码从下标为 0 开始。
def getNext(T):
    # 后缀匹配指向
    i = 0
    # 前缀匹配指向
    j = -1
    # 初始化 next 数组
    next = [-1] * len(T)

    # 此处 next[0] = -1，所以只需要求剩下的 len(T)-1 个即可
    while i < len(T)-1:
        # j == -1 就是找无可找 or 匹配成功，相同前缀长度增加1
        if j == -1 or T[i] == T[j]:
            i += 1
            j += 1
            next[i] = j
        # 匹配不成功则在前面的子串中继续搜索，直至找不到（即 j== -1 的情况）
        else:
            j = next[j]

    return next
```



## 匹配操作实现

那求出了 next 数组，我们就来拆分 KMP 模式串匹配的实现。

在这里主串用 S = abcabcabcabda，模式串用上文中求出的 T = abcabd 为例(**示例默认数组从下标为 1 开始**)。

<div align=center>

![3d9002f8cca85c43671bd44d8906f02](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104850896_0.jpg)

</div>

从上图可以看出，第 1 次在 i = 4，j = 4 时出现不匹配，而此时 next 的值为 2。

这就意味着，第 2 次，从模式串下标为 2 的位置和主串的当前位置的元素开始匹配，形式如下图。

<div align=center>

![d3afc9c2b8a7df50838141ea79ce82d](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104928752_0.jpg)

</div>

上图发现还是不匹配，此时 next 的值为 1，这就意味着，第 3 次从模式串下标为 1 的位置和主串的当前位置的元素进行匹配，形式如下图。

<div align=center>

![30a59603938381186ed54ada7a60b52](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_104952069_0.jpg)

</div>

然后开始的第 3 次。

<div align=center>

![67084b73379b772f890d96eff2439b0](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_105019236_0.jpg)

</div>

看上图，第 3 次在 i = 6，j = 3 的时候出现不匹配，此时的 next 值为 1，那就意味着第 4 次是从模式串 j = 1 的位置与主串的当前位置进行匹配，形式如下图。

<div align=center>

![856770c51f80290ab64d4074b6fd559](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_105109265_0.jpg)

</div>

此时模式串和主串上的元素还是不匹配，此时 next 为 0，**当 next 值为 0 时，相当于是模式串的当前元素和主串的下一个元素进行比较**，如下图。

<div align=center>

![0d27246abf2a8cba728199933d09145](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_105129654_0.png)

</div>

然后有进行了第 5 次，发现匹配成功。

<div align=center>

![fe01d3e7ccb288177f180091d6ba6ef](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_105152271_0.jpg)

</div>

```Python
# 代码从下标为 0 开始
def KMP(S, T):
    i = 0
    j = 0
    next = getNext(T)
    while i < len(S) and j < len(T):
        #j == -1 找无可找，从 S[i+1] 开始和 T[0] 匹配 or 当匹配成功时，往下匹配。
        if j == -1 or S[i] == T[j]:
            i += 1
            j += 1
        # 匹配不成功则用 next(j) 找下一次匹配的位置
        else:
            j = next[j]
    # 如果模式串在主串中存在
    if j == len(T):
        return i - j
    else:
        return -1
```



---

KMP 的讲解到这就全部结束了，真不容易呀，写了好久好久好久。

我自认为 KMP 全部讲清楚了，看懂了保你不管是初学，或者考试、面试都过过过。

你看，KMP 也没那么难学嘛！

**提醒一下**：我在图解 next 和 KMP 原理的时候用的是数组从 1 开始，建议大家在草稿纸上用我的思路，手动模拟一下数组从 0 开始的情况。

毕竟我给出的代码，数组是从下标为 0 开始的，我就是想看看你们是不是真的懂了。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220112_105244010_0.jpg" alt="0e88c63158d2916cb5ef1300b07c4ee" style="zoom:67%;" />

</div>

用心良苦第一人了。

关于 KMP 的代码也不是很长，建议大家根据原理写一套自己的 KMP 板子，到时候用的时候直接拿出来套就好了。
