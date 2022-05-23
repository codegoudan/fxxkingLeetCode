<div align=center>

![jzoffer05-0](https://cdn.codegoudan.com/img/jzoffer05-0.png)

</div>

# 剑指 Offer 05：替换空格

题目链接：[替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)



## 题意

把字符串 s 中的每个空格替换成 "%20"



## 示例

输入：s = "We are happy."

输出："We%20are%20happy."



## 提示

- 0 <= length(s) <= 10000



# 题目解析

水题，难度简单。

字符串这边的题大多数都是可以用编程语言自带的库函数解决的，但是我们在 LeetCode 上刷题初期显然不能这样。

**我们现在刷题的目的是为了更好的理解数据结构与算法，更好的理解一些封装起来的库函数是怎么实现的，而不是简简单单的为了刷题而刷题。**

这道题的解法也不复杂，就是遍历整个字符串 s：

- 碰到非空格就继续走。
- 碰到空格就替换。



# 图解

以 s = "We are happy." 为例。为了方便表示，我以下面的形式表示整个字符串。

<div align=center>

![jzoffer05-1](https://cdn.codegoudan.com/img/jzoffer05-1.png)

</div>

在这维护一个结果列表 res，遍历字符串，刚开始碰到的是 "we"，直接加入 res。

<div align=center>

![jzoffer05-2](https://cdn.codegoudan.com/img/jzoffer05-2.png)

</div>

```Python
if c != ' ':
    res.append(c)
```

接下俩碰到空格，替换为 "%20"，加入列表。

<div align=center>

![jzoffer05-3](https://cdn.codegoudan.com/img/jzoffer05-3.png)

</div>

```Python
if c == ' ':
    res.append('%20')
```

接下来依次按照上面的情况进行，直至列表遍历完。

<div align=center>

![jzoffer05-4](https://cdn.codegoudan.com/img/jzoffer05-4.png)

</div>

本题的解法从左到右遍历整个字符串，所以**时间复杂度为 O(n)**。

时间复杂度还是按照你用的编程语言来算，如果字符串可修改的编程语言，比如 Python、Java，需要维护一个新的列表，这个的**空间复杂度为 O(n)**，像可原地修改字符串的编程语言，比如 C++，这样的**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def replaceSpace(self, s: str) -> str:
        # python 字符串不可修改，所以维护一个列表
        res = []

        for c in s:
            if c == ' ':
                res.append('%20')
            else:
                res.append(c)

        return ''.join(res)
```



## Java 代码实现

```Python
class Solution {
    public String replaceSpace(String s) {
        StringBuilder res = new StringBuilder();
        for(Character c : s.toCharArray())
        {
            if(c == ' ') res.append("%20");
            else res.append(c);
        }
        return res.toString();
    }
}
```


