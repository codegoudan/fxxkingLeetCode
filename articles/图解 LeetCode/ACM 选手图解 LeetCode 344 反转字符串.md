**反转字符串**，一道简单到扣脚的题。

简单到什么地步咧？

我看到这么一句话：真是真正意义上能让我重拳出击的第一道题。

不得不说，看到这我还是有受到侮辱的感觉的。

这也太厉害了吧，我重拳出击的第一道题才是“Hello World”。

<div align=center>

![344-8](https://cdn.codegoudan.com/img/344-8.jpg)

</div>

那么，为啥这么简单我还要写这道题呢？

先不告诉你，往下看就完事了。

<div align=center>

![344-0](https://cdn.codegoudan.com/img/344-0.png)

</div>



# LeetCode 344：反转字符串

题目链接：[反转字符串](https://leetcode-cn.com/problems/reverse-string/)



## 题意

编写一个函数，将输入的字符串反转过来。



## 示例

输入：s = ["h", "e", "l", "l", "o"]

输出：["o", "l", "l", "e", "h"]



## 提示

必须原地修改输入数组，使用 O(1) 的额外空间解决这一问题。

- 1 <= s.length <= 10^5
- s[i] 都是 ASCII 码表中的可打印字符。



# 题目解析

水题，难度简单。

就是将 s[0] s[1] ... s[n-1] 以 s[n-1] ... s[1] s[0] 的形式输出，即 s[i] 和 s[n - i - 1] 交换位置。

那我们可以使用**双指针**的方式解决这道问题，维护一个左指针 left 和右指针 right。

初始化 left 指向数组首元素，right 指向数组右元素。

当 left < right 的时候，交换 s[left] 和 s[right]，同时 left 右移，right 左移。

题目解析说到这，反转字符串不是关键，重要的是我想说...

<div align=center>

![344-1](https://cdn.codegoudan.com/img/344-1.jpg)

</div>

我猜很多小婊贝在做这道题的时候一行代码就解决了这道反转字符串。

毕竟对于想 Python、Java 这种编程语言，是自带很多的库函数，尤其像字符串这种在实际工程中经常要处理的玩意儿，库函数那是数不胜数。

像什么反转字符串，我 Python 直接 reversed 一下多酷多省事，操作多骚，我还用啥双指针，有毛病？

其实这恰恰是我想要你不要做的。你要知道我们现在在这练习，在这刷题的目的是什么。

只是为了完成这道题，提交以后 AC 了就完事了么？

不是。如果你这样想你就错了。

**我们的目的是为了学会，学到根子里去。**

像这种直接用库函数就能做出来的题，**我希望你回归本真，去用你自己想的解决方式去解决这个问题，这样才能慢慢加深你对数据结构和算法的理解，这样最后对你的好处是巨大的。**

言止于此，希望你能听进去，希望你一定要听进去。

<div align=center>

![344-2](https://cdn.codegoudan.com/img/344-2.gif)

</div>



# 图解

以 s = ["h", "e", "l", "l", "o"] 为例，首先初始化双指针。

<div align=center>

![344-3](https://cdn.codegoudan.com/img/344-3.png)

</div>

```python
# 初始化双指针
left = 0
right = len(s) - 1
```

第 1 步，left = 0，right = 4，left < right，交换两个元素。

<div align=center>

![344-4](https://cdn.codegoudan.com/img/344-4.png)

</div>

```Python
while left < right:
    s[left], s[right] = s[right], s[left]
```

left 向右移动 1，right 向左移动 1，此时 left = 1，right = 3。

```Python
left += 1
right -= 1
```

<div align=center>

![344-5](https://cdn.codegoudan.com/img/344-5.png)

</div>

第 2 步，left < right，交换两个元素。

<div align=center>

![344-6](https://cdn.codegoudan.com/img/344-6.png)

</div>

left 向右移动 1，right 向左移动 1，此时 left = right，结束。

<div align=center>

![344-7](https://cdn.codegoudan.com/img/344-7.png)

</div>

返回字符串 s = ["o", "l", "l", "e", "h"]。

本题解一共执行了 n/2 次操作，所以**时间复杂度为 O(n)**。

除此以外只用了额外的两个指针存放变量，所以**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        # 初始化双指针
        left = 0
        right = len(s) - 1

        # 这种方法可以不用判断元素奇偶
        while left < right:
            s[left], s[right] = s[right], s[left]
            
            #交换后，左指针右移，右指针左移
            left += 1
            right -= 1
```



## Java 代码实现

```java
class Solution {
    public void reverseString(char[] s) {
        int n = s.length;
        for (int left = 0, right = n - 1; left < right; ++left, --right) {
            char tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
        }
    }
}
```



