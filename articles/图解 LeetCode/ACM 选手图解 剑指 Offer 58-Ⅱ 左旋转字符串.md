**左旋转字符串**，光看题目觉的可能是道挺狠的题。

其实就是纸老虎，不禁打，直接搞起！

<div align=center>

![8ae2fe011263ff1920fad8d01440e43](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_225627880_0.jpg)

</div>



# 剑指 Offer58-Ⅱ：左旋转字符串

题目链接：[左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)



## 题意

实现一个字符串左旋转操作。

左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。



## 示例

输入：s = "abcdefg", k = 2
输出："cdefgab"



## 提示

1 <= k < s.length <= 10000



# 题目解析

水题，难度简单，其实看懂题就会了。

所有的代码尽量都是自己写的，不要用编程语言自带的库函数。

还是想重复，对于字符串这部分，前期刷题所有的操作最好都是自己老老实实实现的。

再回到这道理，其实**就是遍历列表，拆开，然后重新拼接的过程**：

这个过程维护一个列表。

- 先将 k+1 到末尾的字符添加到列表中。
- 再将 0 到 k 的字符添加到列表中。



# 图解

在这以 s = "abcdefg", k = 2 为例，首先初始化一个列表：

<div align=center>

![41c0bf17699369224151d69e001c1e5](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_225639439_0.jpg)

</div>

此例 k = 2，那根据题目解析中说的，先将 k+1 到末尾的添加到 res 中。

<div align=center>

![e5f06bf3798b970f7ee5c680a3cb05f](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_225649677_0.jpg)

</div>

之后将 0 至 k-1 的字符添加至末尾。

<div align=center>

![63fbf4e64d05702884fb40204db8701](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_225659811_0.jpg)

</div>

本题的解题方法从左至右遍历整个字符串，所以**时间复杂度为 O(n)**。

时间复杂度还是按照你用的编程语言来算，如果字符串可修改的编程语言，比如 Python、Java，需要维护一个新的列表，这个的**空间复杂度为 O(n)**，像可原地修改字符串的编程语言，比如 C++，这样的**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        # python 字符串不可修改，所以维护一个列表
        res = []

        # 首先添加 n+1 到 len(s) 的元素
        for i in range(n, len(s)):
            res.append(s[i])

        # 再添加 0 到 n 的元素
        for i in range(n):
            res.append(s[i])

        return ''.join(res)
```



## Java 代码实现

```Python
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder res = new StringBuilder();
        for(int i = n; i < s.length(); i++)
            res.append(s.charAt(i));
        for(int i = 0; i < n; i++)
            res.append(s.charAt(i));
        return res.toString();
    }
}
```



---

好啦，**图解左旋转字符串**到这就结束辣。

你看像这种看起来花里胡哨的题目，完全不用慌，都是小猫咪。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_225714296_0.jpg" alt="6bff714c82164c0de16f115b23d7062" style="zoom:80%;" />

</div>