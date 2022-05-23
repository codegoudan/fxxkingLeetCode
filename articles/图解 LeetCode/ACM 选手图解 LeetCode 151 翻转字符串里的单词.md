**翻转字符串中的单词**，估计很多小婊贝一看到这个题，脑子里第一反应就是水题。

因为很多的编程语言都自带有库函数，几行代码就可以解决。

但是不行呀，这么玩儿这道难度中等的题，水题的帽子就带实了。

**刷题初期还是要老老实实，少用现成的东西，自己老老实实的实现代码，编写对应的函数。**

话至于此，接下来我们来开开心心肝题。

<div align=center>

![151-0](https://cdn.codegoudan.com/img/151-0.png)

</div>

# LeetCode 151 翻转字符串里的单词

题目链接：[翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)



## 题意

逐个翻转字符串 s 中的所有单词。

单词是由非空格字符组成的字符串，s 中使用至少一个空格将字符串中的单词分隔开。

返回一个翻转 s 中单词顺序并用单个空格相连的字符串。



## 示例

输入：s = "the sky is blue"
输出："blue is sky the"

输入：s = " hello world  "

输出："world hello"

解释：输入字符串可以在前面或者后面包含多余的空格，但是翻转后的字符不能包括。



## 提示

- 1 <= s.length <= 10^4
- s 包括英文大小写字母、数字和空格 ' '。
- s 中至少存在一个单词。



# 题目解析

翻转字符串里的单词，难度可简单可中等。

这道题还是开头说的，用编程语言自带的库函数去解决，那就是一道几行代码的简单水题，这道题也就失去了意义。

为了配上这道题难度中等的身份，大家还是老老实实实自己动手实现对应函数。

毕竟，写代码是件快乐的事。

<div align=center>

![151-1](https://cdn.codegoudan.com/img/151-1.jpg)

</div>

回到这道题本身上来，你想我们之前做过的**反转字符串**和**反转字符串****Ⅱ**这俩，没看过的可以看下：



**[反转字符串不是关键，重要的是我想说...](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475920931&idx=1&sn=cf73562d5c06df758f75d22a5e3a7e13&chksm=ff22e9aec85560b84c7aa861dbff4038007a530109d9c6bd698493c7314e9787a3cca5ba9231&scene=21#wechat_redirect)**

**[这次反转的字符串，不简单。](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475920952&idx=1&sn=d40fa57affc68367a663858c652e4f9a&chksm=ff22e9b5c85560a38e66bde6197041168b8782148b07fa70ee251817c36e3c261fde0c1aefa8&scene=21#wechat_redirect)**



其实就是整体反转，再局部反转，反反得正，最后就解出来了。

不过说实话，**这道题还是考察了挺多字符串的操作的**，加上题目本身加的限制，**整道题分 3 步解决**：

1. 去除多余空格。
2. 反转整个字符串。
3. 反转每个单词。

具体实现，接下来看图解。



# 图解

以 s = " hello world  " 为例。根据题目解析中所说，图解分为 3 步。

<div align=center>

![151-2](https://cdn.codegoudan.com/img/151-2.png)

</div>

```Python
# 初始化双指针
left, right = 0, len(s) - 1
```



## 第一步：去除多余空格。

去除多余空格总体来说是 **3 个位置：开头、结尾、字符串内**。

(1) 去除开头空格

左指针向右跑，碰到第一个不是空格的字符停下。

<div align=center>

![151-3](https://cdn.codegoudan.com/img/151-3.png)

</div>

```Python
# 去除开头的空格
while left < right and s[left] == ' ':
    left += 1
```

(2) 去除结尾空格

右指针像左跑，碰到第一个不是空格的字符停下。

<div align=center>

![151-4](https://cdn.codegoudan.com/img/151-4.png)

</div>

```Python
# 去除结尾的空格
while left < right and s[right] == ' ':
    right -= 1
```

(3) 去除字符串内多余空格

左指针向右跑，如果当前是字符，就往前跑，如果当前是空格，如果前一个不是空格，就往前跑，如果前一个也是空格，那这个空格就删掉，因为多余了。

<div align=center>

![151-5](https://cdn.codegoudan.com/img/151-5.png)

</div>

```Python
# 去除单词间多余的空格
while left <= right:
    if s[left] != ' ':
        new_s.append(s[left])
    # 如果当前是空格，且前一个字符不是空格，则添加
    elif s[left] == ' ' and new_s[-1] != ' ':
        new_s.append(s[left])
    left += 1
```



## 第二步：反转整个字符串

反转字符串大家都很熟了，就是直接把字符串反转过来。

<div align=center>

![151-6](https://cdn.codegoudan.com/img/151-6.png)

</div>

```Python
# 反转字符串
def reverseString(self, s):
    # 初始化双指针
    left, right = 0, len(s) - 1
    # 这种方法可以不用判断元素奇偶
    while left < right:
        s[left], s[right] = s[right], s[left]
        # 交换后，左指针右移，右指针左移
        left += 1
        right -= 1
    return s
```



## 第三个：反转每个单词

首先初始化指向每个单词前后的指针。

<div align=center>

![151-7](https://cdn.codegoudan.com/img/151-7.png)

</div>

```Python
# 初始化指向每个单词前后的指针
left, right = 0, 0
n = len(s)
```

之后右指针往右跑，跑完整个单词，然后单词反转。

<div align=center>

![151-8](https://cdn.codegoudan.com/img/151-8.png)

</div>

```Python
whileleft < n:
    while right < n and s[right] != ' ':
        right += 1
    s[left : right] = self.reverseString(s[left : right])
```

反转完一个单词，接着反转下一个单词。

<div align=center>

![151-9](https://cdn.codegoudan.com/img/151-9.png)

</div>

本题的解决方式的**时间复杂度显然为 O(n)**。

至于空间复杂度依照你所用的编程语言来确定，因为有的编程语言存在字符串不可修改的情况，比如 Python、Java，这就需要额外用一个数组来存储，那么它的**空间复杂度就成了 O(n)**。

如果所用编程语言字符串支持修改，比如 C++，那**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    # 去除多余空格
    def removeSpaces(self, s):
        # 初始化双指针
        left, right = 0, len(s) - 1

        # 去除开头的空格
        while left < right and s[left] == ' ':
            left += 1
        # 去除结尾的空格
        while left < right and s[right] == ' ':
            right -= 1
        # new_s 存储去掉多余空格剩下的东西
        new_s = []
        # 去除单词间多余的空格
        while left <= right:
            if s[left] != ' ':
                new_s.append(s[left])
            # 如果当前是空格，且前一个字符不是空格，则添加
            elif s[left] == ' ' and new_s[-1] != ' ':
                new_s.append(s[left])
            left += 1

        return new_s

    # 反转字符串
    def reverseString(self, s):
        # 初始化双指针
        left, right = 0, len(s) - 1

        # 这种方法可以不用判断元素奇偶
        while left < right:
            s[left], s[right] = s[right], s[left]
            # 交换后，左指针右移，右指针左移
            left += 1
            right -= 1
        return s

    # 反转每个单词
    def reverseEachword(self, s):
        # 初始化指向每个单词前后的指针
        left, right = 0, 0
        n = len(s)

        while left < n:
            while right < n and s[right] != ' ':
                right += 1
            s[left : right] = self.reverseString(s[left : right])
            # 反转完一个单词，该反转下个单词了
            left = right + 1
            right += 1
        return s

    def reverseWords(self, s: str) -> str:
        # 第 1 步：去除空格
        s = self.removeSpaces(s)
        # 第 2 步：反转字符串
        s = self.reverseString(s)
        # 第 3 步：反转每个单词
        s = self.reverseEachword(s)
        # 第 4 步：输出翻转后的字符串
        return ''.join(s)
```



## Java 代码实现

```Python

class Solution {
    public String reverseWords(String s) {
        // 1.去除首尾以及中间多余空格
        StringBuilder sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    private StringBuilder removeSpace(String s) {
        int start = 0;
        int end = s.length() - 1;
        while (s.charAt(start) == ' ') start++;
        while (s.charAt(end) == ' ') end--;
        StringBuilder sb = new StringBuilder();
        while (start <= end) {
            char c = s.charAt(start);
            if (c != ' ' || sb.charAt(sb.length() - 1) != ' ') {
                sb.append(c);
            }
            start++;
        }
        return sb;
    }

    /**
     * 反转字符串指定区间[start, end]的字符
     */
    public void reverseString(StringBuilder sb, int start, int end) {
        while (start < end) {
            char temp = sb.charAt(start);
            sb.setCharAt(start, sb.charAt(end));
            sb.setCharAt(end, temp);
            start++;
            end--;
        }
        int end = 1;
        int n = sb.length();
        while (start < n) {
            while (end < n && sb.charAt(end) != ' ') {
                end++;
            }
            reverseString(sb, start, end - 1);
            start = end + 1;
            end = start + 1;
        }
    }
}
```



---

好啦，**图解翻转字符串中的单词**到这就结束辣。

搞清楚了题意然后一步步的写还蛮有意思的，整点 bug，搞搞自己。

这成就感不比直接调用几个库函数直接实现出来爽的多？

**写代码真是这个世界上最有意思的事儿。**

<div align=center>

![151-10](https://cdn.codegoudan.com/img/151-10.jpg)

</div>