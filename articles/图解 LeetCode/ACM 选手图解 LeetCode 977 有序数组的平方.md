**有序数组的平方**，同样是使用**双指针法**的经典题。

不同于【[**LeetCode27 移除元素**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475922234&idx=1&sn=5f0a3fce40927c80ff5608da90b6fcd0&chksm=ff22f4b7c8557da10186decdfd4b1f57277b44e1f452388be2ef407bd5e91f8b2e24c624fceb&scene=21#wechat_redirect)】的快慢指针，这次的双指针是一种另外的用法。

<div align=center>

![42712bdf8cc67cc5e548407d7f12823](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_182404018_0.jpg)

</div>



# LeetCode 977：有序数组的平方

题目链接：[有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)



## 题意

一个按非递减顺序排序的整数数组 nums，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。



## 示例

输入：nums = [-4,-1,0,3,10]

输出：[0,1,9,16,100]

解释：平方后，数组变为 [16,1,0,9,100]，排序后，数组变为 [0,1,9,16,100]



## 提示

- 1 <= len(nums) <= 10^4
- -10^4 <= nums[i] <= 10^4
- nums 已按非递减顺序排序



# 题目解析

水题，难度简单。

现在我还没讲到排序，如果小婊贝们知道快排，这道题最无脑暴力的方法其实就是每个元素平方完了，再快排一下。

这个过程就涉及到两步：

- 第 1 步：一层 for 循环遍历数组，将各元素平方后存入数组，这一步的时间复杂度为 O(n)。
- 第 2 步：快排，时间复杂度为 O(nlogn)。

总的时间复杂度显然是 O(n + nlogn)，这个复杂度说实话稍微有点高了。

**我们要善于使用题意来选择最合适的办法。**

<div align=center>

![50e6bffc50459852b4b0e2ffe8d556d](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_182631733_0.jpg)

</div>

数组 nums 是非递减排序的数组，那就相当于是个递增的数组，且每个元素的取值 -10^4 <= nums[i] <= 10^4，这就证明元素值有正有负。

通过这两个条件其实我们可以得出，**平方以后的最大值肯定出现在两侧，不是左边就是右边（负数的平方为正数）**。

碰到这种情况，我们一般祭出**双指针法**来解决，left 指向下标 0，right 指向下标 n - 1：

- 新建一个结果数组 res 存储最后的结果，site 指向数组末尾，数组从后向前存储。
- 若 nums[left] * nums[left] < nums[right] * nums[right]，res[site] = nums[right] * nums[right]。
- 若 nums[left] * nums[left] >= nums[right] * nums[right]，res[site] = nums[left] * nums[left]。



# 图解

以 nums = [-4,-1,0,3,10] 为例。

首先初始化 left 和 right 指针，新建 res 数组，site 指针指向 res 数组的尾部。

<div align=center>

![73ef186435c8f954ac1bc4c5f444bca](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_182723768_0.jpg)

</div>

```Python
# 初始化双指针
left = 0
right = len(nums) - 1
# 存储结果数组，从数组末尾开始存储
res = [-1] * len(nums)
site = len(nums) - 1
```

第 1 步，left = 0，right = 4，site = 4：

此时 nums[left] * nums[left] = 16 < nums[right] * nums[right] = 100，则 res[site] = nums[right] * nums[right] = 100，同时 right 向左移动一位，site 向左移动一位。

<div align=center>

![458290bce4174de30b6eb1f64061460](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_182806296_0.jpg)

</div>

```Python
if nums[left] * nums[left] < nums[right] * nums[right]:
    res[site] = nums[right] * nums[right]
    right -= 1
site -= 1
```

第 2 步，left = 0，right = 3，site = 3：

此时 nums[left] * nums[left] = 16 > nums[right] * nums[right] = 9，则 res[site] = nums[left] * nums[left] = 16，同时 left 向右移动一位，site 向左移动一位。

<div align=center>

![a98c84fba32dc9cdc19b5fe531df60c](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_182843373_0.jpg)

</div>

```Python
if nums[left] * nums[left] >= nums[right] * nums[right]:
    res[site] = nums[left] * nums[left]
    letf += 1
site -= 1
```

第 3 步，left = 1，right = 3，site = 2：

此时 nums[left] * nums[left] = 1 < nums[right] * nums[right] = 9，则 res[site] = nums[right] * nums[right] = 9，同时 right 向左移动一位，site 向左移动一位。

<div align=center>

![fa3a5089913f2f9f37bb02a1568b187](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_183036237_20.jpg)

</div>

第 4 步，left = 1，right = 2，site = 1：

此时 nums[left] * nums[left] = 1 > nums[right] * nums[right] = 0，则 res[site] = nums[left] * nums[left] = 1，同时 left 向右移动一位，site 向左移动一位。

<div align=center>

![32f3abca153dc0ec1d91d104d2d063b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_183045380_0.jpg)

</div>

**这里就要注意我在代码中特别强调的：**

```Python
# 注意这里是 <=
while left <= right:
```

第 5 步，left = 2，right = 2，site = 0：

此时 nums[left] * nums[left] = 0 >= nums[right] * nums[right] = 0，则 res[site] = nums[left] * nums[left] = 0。

循环就此结束，输出结果 res。

<div align=center>

![b579e2bf0d2d2879e123c8f18ebff28](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_183128641_0.jpg)

</div>

本道题的解法，相当于遍历了一遍数组，**时间复杂度为 O(n)**。

因为额外维护了一个长度为 n 的 res 结果数组，所以**空间复杂度也为 O(n)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:

        if len(nums) == 1:
            return [nums[0] * nums[0]]

        # 初始化双指针
        left = 0
        right = len(nums) - 1

        # 存储结果数组，从数组末尾开始存储
        res = [-1] * len(nums)
        site = len(nums) - 1

        # 注意这里是 <=
        while left <= right:
            # 从两端遍历，将平方数组大得存储在 res 数组中
            if nums[left] * nums[left] < nums[right] * nums[right]:
                res[site] = nums[right] * nums[right]
                right -= 1
            else:
                res[site] = nums[left] * nums[left]
                left += 1

            site -= 1

        return res
```



## Java 代码实现

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int right = nums.length - 1;
        int left = 0;
        int[] res = new int[nums.length];
        int index = res.length - 1;
        while (left <= right) {
            if (nums[left] * nums[left] > nums[right] * nums[right]) {
                res[index--] = nums[left] * nums[left];
                ++left;
            } else {
                res[index--] = nums[right] * nums[right];
                --right;
            }
        }
        return res;
    }
}
```


