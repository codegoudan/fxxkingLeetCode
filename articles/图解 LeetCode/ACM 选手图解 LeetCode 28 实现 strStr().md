**实现 strStr()**，KMP 算法实战第一题。我们通过这道题来熟悉一下 KMP 算法的使用。

<div align=center>

![aa84c33897106937391a4fd0a9a82fd](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_200827438_0.jpg)

</div>



# LeetCode 28：实现 strStr()

题目链接：[实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)



## 题意

给你两个字符串 haystack 和 needle，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。

如果不存在则返回 -1。



## 示例

输入：haystack = “hello”，needle = “ll”。
输出：2



## 提示

- 0 <= len(haystack)、len(needle) <= 5 * 10^4。
- haystack 和 needle 仅由小写英文字符组成



# 题目解析

水题，难度简单。

题意很简单，就是找模式串在主串中的位置，haystack 为主串，needle 为模式串。

解决此类匹配问题是 KMP 算法的常规操作。

如果还不了解 KMP 算法，可以先去看下面这篇文章：



[**ACM 选手带你玩转 KMP 算法！**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924907&idx=1&sn=6f6fc1475be2d7d2ca5ab6e0ec755bca&chksm=ff22fa26c8557330a906f6ed9f444d71064a590109b093d8e97f0ab1cd82e5106a5138e8aecd&scene=21#wechat_redirect)



既然是用 KMP，那题解步骤分为**两步**：

- 先求模式串的 next 数组。
- 后进行匹配操作。



# 图解

以 haystack = “hello”，needle = “ll” 为例。



## 第一步求模式串 needle 的 next 数组。

下图是模式串 needle 的各个前缀：

<div align=center>

![97242b6b2be02293706565dd0cdc406](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201135074_0.jpg)

</div>

根据我在文章【[**KMP 算法**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924907&idx=1&sn=6f6fc1475be2d7d2ca5ab6e0ec755bca&chksm=ff22fa26c8557330a906f6ed9f444d71064a590109b093d8e97f0ab1cd82e5106a5138e8aecd&scene=21#wechat_redirect)】的图解，模式串 needle 的 next 数组为：

<div align=center>

![3415728b65690ab1d1586b525c5a031](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201213122_0.jpg)

</div>

```Python
def getNext(self, needle: str):
    # 后缀匹配指向
    i = 0
    # 前缀匹配指向
    j = -1
    # 初始化 next 数组
        next = [-1] * len(needle)
    # 此处 next[0] = -1，所以只需要求剩下的 len(T)-1 个即可
    while i < len(needle) - 1:
        # j == -1 就是找无可找 or 匹配成功，相同前缀长度增加1
        if j == -1 or needle[i] == needle[j]:
            i += 1
            j += 1
            next[i] = j
        # 匹配不成功则在前面的子串中继续搜索，直至找不到（即 j== -1 的情况）
        else:
                j = next[j]
    return next
```



## 第二步进行匹配操作

(1) 初始化主串 i 、模式串 j 指针以及 next 数组。

<div align=center>

![096b84033536045f96993522685943c](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201336007_0.jpg)

</div>

```Python
i = 0
j = 0
next = self.getNext(needle)
```

(2) i =0，j = 0， haystack[i] != needle[j]，此时令 j = next[j] = -1。

<div align=center>

![d3892aa963528796651adf68aff46c1](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201406830_0.jpg)

</div>

当 j = -1 时，代表着找无可找，所以 i 和 j 同时向右移动。

<div align=center>

![0cb459141464588c9ec5892afc4375a](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201422080_0.jpg)

</div>

```Python
# j == -1 就是找无可找 or 匹配成功，相同前缀长度增加1
if j == -1 or haystack[i] == needle[j]:
    i += 1
    j += 1
# 匹配不成功则用 next(j) 找下一次匹配的位置
else:
    j = next[j]
```

(3) i =1，j = 0， haystack[i] != needle[j]，此时令 j = next[j] = -1。

<div align=center>

![cad438c7850d7cb365ad6e2c617cc05](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201457315_0.jpg)

</div>

j = -1，找无可找，i 和 j 继续向右移动。

<div align=center>

![729924737e91a2c3478bbe1572ef46e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201532332_0.jpg)

</div>

(3) i =2，j = 0， haystack[i] == needle[j]，i 和 j 向右移动。

<div align=center>

![af4515f83af3a01383fb064ac66ab6e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201745786_0.jpg)

</div>

(4) i =3，j = 1， haystack[i] == needle[j]，i 和 j 向右移动。

<div align=center>

![515a55f13d82749e1fb70cbde15afb8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201808967_0.jpg)

</div>

(5) i =4，j = 2， 此时 j > len(needle)，跳出循环。

此时 j == len(needle)，证明模式串 needle 在主串 haystack 中存在，返回模式串在主串中出现的第 1 个位置，即 i - j。

```Python
# 如果模式串在主串中存在
if j == len(needle):
    return i - j
```

假设主串长度为 n，模式串长度为 m。

本题解使用 KMP 算法，**时间复杂度为 O(n + m)**。

因为额外维护了一个 next 数组，所以**空间复杂度为 O(m)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def getNext(self, needle: str):
        # 后缀匹配指向
        i = 0
        # 前缀匹配指向
        j = -1
        # 初始化 next 数组
        next = [-1] * len(needle)

        # 此处 next[0] = -1，所以只需要求剩下的 len(T)-1 个即可
        while i < len(needle) - 1:
            # j == -1 就是找无可找 or 匹配成功，相同前缀长度增加1
            if j == -1 or needle[i] == needle[j]:
                i += 1
                j += 1
                next[i] = j
            # 匹配不成功则在前面的子串中继续搜索，直至找不到（即 j== -1 的情况）
            else:
                j = next[j]

        return next

    def strStr(self, haystack: str, needle: str) -> int:
        i = 0
        j = 0
        next = self.getNext(needle)
        while i < len(haystack) and j < len(needle):
            # j == -1 找无可找，从 S[i+1] 开始和 T[0] 匹配 or 当匹配成功时，往下匹配。
            if j == -1 or haystack[i] == needle[j]:
                i += 1
                j += 1
            # 匹配不成功则用 next(j) 找下一次匹配的位置
            else:
                j = next[j]
        # 如果模式串在主串中存在
        if j == len(needle):
            return i - j
        else:
            return -1
```



## Java 代码实现

```Java
class Solution {
    private void getNext(String p, int next[]) {
        int len = p.length();
        int i = 0;
        int j = -1;
        next[0] = -1;
        while (i < len - 1) {
            if (j == -1 || p.charAt(i) == p.charAt(j)) {
                i++;
                j++;
                next[i] = j;
            } else{
                j = next[j];
            }
        }
    }
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0)
            return 0;
        int i = 0;
        int j = 0;
        int[] next = new int[needle.length()];
        getNext(needle, next);
        while (i < haystack.length() && j < needle.length()) {
            if (j == -1 || haystack.charAt(i) == needle.charAt(j)) {
                i++;
                j++;
            } else {
                j = next[j];
            }
            if (j == needle.length())
                return i - j;
        }
        return -1;
    }
}
```



---

**图解实现 strStr()** 到这就结束辣，是不是感觉还可以？

**这道题的图解我模拟的是数组从下标 0 开始的**，所以如果你还因为我在【**[KMP 算法](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924907&idx=1&sn=6f6fc1475be2d7d2ca5ab6e0ec755bca&chksm=ff22fa26c8557330a906f6ed9f444d71064a590109b093d8e97f0ab1cd82e5106a5138e8aecd&scene=21#wechat_redirect)**】中说的下面这句话而准备打我的话...

<div align=center>

![a796ef5fa50276c41202ee69fe08176](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_201954880_0.jpg)

</div>

希望你看了这道题放下手里的刀...

<div align=center>

![2c89cf72745d70d904297aa3eb452ca](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_202012163_0.jpg)

</div>

我也希望小宝贝们是真的懂了，毕竟 KMP 这玩意懂了就没啥好害怕的。
