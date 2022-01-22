<div align=center>

![52f498ca57973794c7cd7b36a344572](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_212906358_0.jpg)

</div>



#  LeetCode 50：Pow(x,n)

题目链接：[Pow(x,n)](https://leetcode-cn.com/problems/powx-n/)



## 题意

实现 pow(x,n)，即计算 x 的 n 次幂函数。



## 示例

输入：x = 2.00000，n = 10

输出：1024.00000

输入：x = 2.00000，n = -2

输出：0.25000



## 提示

- -100.0 <= x <= 100.0
- -2^31 <= n <= 2^31 - 1
- -10^4 <= x^n <= 10^4



# 题目解析

难度中等，针对这道题，有很多种经典解法，分治算法是其中一种。

如果还不了解分治算法，可以先看下面这篇文章：



**[ACM 选手带你玩转分治算法！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475925424&idx=1&sn=b83a152e2d86edcc5eef9005c99aa9ba&chksm=ff22f83dc855712b66732098e90f93f79c3a1c050c06ffdc9c312f8e858a30de08468093d2da&scene=21#wechat_redirect)**



**分治算法由“分”和“治”两部分组成**，但是它主要包括 **3 个过程**：

- 划分（Divide）
- 求解（Conquer）
- 合并（Combine）

其中：

**划分（Divide）**：将原问题划分为规模较小的子问题，子问题相互独立，与原问题形式相同。

**求解（Conquer）**：递归的求解划分之后的子问题。

**合并（Combine）**：这一步非必须。有些问题涉及合并子问题的解，将子问题的解合并成原问题的解。有的问题则不需要，只是求出子问题的解即可。

<div align=center>

![4bf5f65f2692cf4df212c0d6ee09efe](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_213115761_0.jpg)

</div>

说白了就是**先找找拆分到最小规模问题时怎么解，然后再瞅瞅随着问题规模增大点问题怎么解，最后就是找到递归函数，码出递归代码即可**。

根据 n 的取值范围，它可能分为 3 种情况：n 为负数、n 为正数时又分为 n 为奇数、n 为偶数。

所以整个的解题步骤就很明确：

**(1) 划分**

划分就是拆解到问题的最小规模，这里可以用一点点二分的思想。

n 为偶数时，x^n 可以转换为 x 的 n/2 相乘，然后 x 的 n/2 又可以转换为 x 的 n/4 相乘，...，直到转换成 x^1。

n 为奇数时，先拆出一个 x，n-1 为偶数，再以上面同样的逻辑去分，只是最后记得把拆出来的 x 带上。

n 为负数时，-n 是正数，x 的 n 次方，相当于就是 x^ (-n) ，按照上面求完以后取个倒数。

**(2) 求解**

递归的求解划分之后的子问题。

最小的情况就是 n = 1 或者 n = 0，此时的值为 x 或者 1。

**(3) 合并**

一步步的向上合并，合并出最后的解。



# 图解

以 x = 2.00000，n = 10 为例。

先是自顶向下的分解问题，分解如下图所示：

<div align=center>

![eb0a30d838507e272687c26d1b75bc9](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_213156797_0.jpg)

</div>

之后按照自底向上的方式合并解。

此解法需要计算 x 的 n 次方，x 的 n/2 次方，x 的 n/4 次方，...，x 的 1 次方，所以**时间复杂度为 O(logn)**。

虽然分治算法没有直接分配额外的数组空间，但是在递归过程中调用了额外的栈空间，分治算法相当于每次将当前问题拆成了两部分，n 在变为 1 之前需要进行 O(logn) 次递归，所以**空间复杂度也为 O(logn)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def myPow(self, x: float, n: int) -> float:

        if n == 0:
            return 1
        if n == 1 or x == 1.0:
            return x
        # 如果 n 为负数，先求 x^n，再取倒数
        if n < 0:
            return 1 / self.myPow(x, -n)
        # 如果 n 为奇数
        if n % 2:
            return x * self.myPow(x, n - 1)
        # 如果 n 为偶数
        return self.myPow(x * x, n / 2)
```



## Java 代码实现

```Java
class Solution {
    public double myPow(double x, int n) {
        if (n == 0){
            return 1;
        }
        if (n == 1 || x == 1.0){
            return x;
        }
        if (n < 0){
            return 1 / x * myPow(1 / x, -(n + 1));
        }
        int a = n / 2;
        int b = n % 2;
        return b == 0 ? myPow(x * x, a) : x * myPow(x * x, a);
    }
}
```



---

**图解 Pow(x,n)** 到这就结束辣，虽然是道中等难度的题，是不是感觉还挺简单的？

要是因此你就觉得分治算法也没啥难的，那你就是 too young too simple。

这种事情知道了答案就很简单，**难的是推导的公式的过程**。

**这个没有捷径，只能多思考多练习**，要加油呀！