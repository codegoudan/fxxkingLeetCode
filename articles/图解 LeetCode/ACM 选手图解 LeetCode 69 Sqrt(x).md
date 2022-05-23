算术平方根 **Sqrt(x)**，通过这道**常见面试题**作为二分查找实战系列开篇。

咱们话不多说，赶紧开整。

<div align=center>

![69-0](https://cdn.codegoudan.com/img/69-0.png)

</div>



# LeetCode 69：Sqrt(x)

题目链接：[Sqrt(x)](https://leetcode-cn.com/problems/sqrtx/)



## 题意

给一个非负整数 x，计算并返回 x 的算术平方根。

返回类型是整数，结果只保留小数部分，小数部分被舍去。



## 示例

输入：x = 8

输出：2

解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。



## 提示

- 不允许使用任何内置指数函数和算符  
- 0 <= x <= 2^31 -1



# 题目解析

水题，难度简单，**面试中的常见题**。

这道题当然不能使用 pow(x, 0.5) 或 x ** 0.5 等内置函数，不然就没了考察的意义。

对于本题，是对非负整数 x 开根号，即 y = sqrt(x)。

<div align=center>

![69-1](https://cdn.codegoudan.com/img/69-1.png)

</div>

这是一个单调递增函数。

而我在【[**ACM 选手带你玩转二分查找！**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475922852&idx=1&sn=f6990bcafef36c96599866ab99bb25f2&chksm=ff22f229c8557b3fab35b99c29038eb4ea0c024010128eb09e8923922666e16e3928b1ed4edf&scene=21#wechat_redirect)】文章中讲过，二分查找的使用，要有一个**前提条件**：**要查找的数必须在一个有序数组里**。

这道题的条件正好满足单调递增，并且 sqrt(x) 返回的是整数 res，而 res 是满足 res * res <= x 的最大值，所以可以用二分查找来解决。

明白了这些剩下的就是常规操作：

- 确定二分查找的上界 high = x 和下界 low = 0。
- 比较 mid * mid 与 x 的关系，动态调整 [low, high]。



# 图解

以 n = 8 为例，为了方便大家理解，我把从 low ~ high 之间所有的数都列出来。

<div align=center>

![69-2](https://cdn.codegoudan.com/img/69-2.png)

</div>

```Python
low, high, res = 1, x, -1
```

第 1 步，x = 8，low = 1，high = 8，mid = (low + high) // 2 = 4：

<div align=center>

![69-3](https://cdn.codegoudan.com/img/69-3.png)

</div>

```Python
mid = (low + high) // 2
```

此时 mid * mid = 16 > x，则 high 向左移动至 mid - 1 = 3 处。

<div align=center>

![69-4](https://cdn.codegoudan.com/img/69-4.png)

</div>

```Python
if mid * mid > x:
      high = mid - 1
```

第 2 步，x = 8，low = 1，high = 3，mid = (low + high) // 2 = 2：

<div align=center>

![69-5](https://cdn.codegoudan.com/img/69-5.png)

</div>

此时 mid * mid = 4 <= x，则 res = mid = 2，low 向右移动至 mid + 1 = 3 处。

<div align=center>

![69-6](https://cdn.codegoudan.com/img/69-6.png)

</div>

```Python
if mid * mid <= x:
    res = mid
    low = mid + 1
```

第 3 步，x = 8，low = 3，high = 3，mid = (low + high) // 2 = 3：

<div align=center>

![69-7](https://cdn.codegoudan.com/img/69-7.png)

</div>

此时 mid * mid = 9 > x，则 high 向左移动至 mid - 1 = 2 处。

此时 low > high，循环终止，输出 res = 2。

本题题解使用二分查找 res，查找的长度为 x，所以**时间复杂度为 O(logx)**。

额外创建了 low、high 指针及 res 存储结果，所以**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution(object):
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x == 0 or x == 1:
            return x

        low, high, res = 1, x, -1

        while low <= high:
            mid = (low + high) // 2
            if mid * mid <= x:
                res = mid
                low = mid + 1
            else:
                high = mid - 1

        return res
```



## Java 代码实现

```Java
class Solution {
    public int mySqrt(int x) {
        int low = 0, high = x, res = -1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if ((long) mid * mid <= x) {
                res = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return res;
    }
}
```


