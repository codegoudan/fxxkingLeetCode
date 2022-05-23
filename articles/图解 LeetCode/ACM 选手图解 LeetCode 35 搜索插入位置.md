**搜索插入位置**，同样作为二分查找的经典题。

这道题简单，但是不容易 AC，LeetCode 上的通过率很低，不足五成。

这就是之前我在【**[二分查找](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475922852&idx=1&sn=f6990bcafef36c96599866ab99bb25f2&chksm=ff22f229c8557b3fab35b99c29038eb4ea0c024010128eb09e8923922666e16e3928b1ed4edf&scene=21#wechat_redirect)**】文章里讲的，**越简单的东西，越在细节上容易栽跟头。**

<div align=center>

![35-0](https://cdn.codegoudan.com/img/35-0.png)

</div>



# LeetCode 35：搜索插入位置

题目链接：[搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)



## 题意

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。

如果目标值不在数组中，返回它将会被按顺序插入的位置。



## 示例

输入: nums = [1,3,5,6], target = 5

输出: 2

输入: nums = [1,3,5,6], target = 0

输出: 0



## 提示

必须使用时间复杂度为 O(logn) 的算法

- 1 <= len(nums) <= 10^4
- -10^4 <= nums[i]、target <= 10^4
- nums 为无重复元素的升序排列数组



# 题目解析

首先看题意，数组 nums 是无重复元素的升序排列数组，而且是必须使用 O(logn) 时间复杂度的算法。

说实话，当时看到这的时候我愣了一下，好家伙，直接报二分查找身份证号得了。

<div align=center>

![35-1](https://cdn.codegoudan.com/img/35-1.jpg)

</div>

知道是二分查找，剩下就是走流程的事，但是这个和平常做的的二分查找还稍微有点不一样。

**普通的二分查找的条件是：数组有序、无重复、能找到。**

这里要二分查找的条件只是：数组有序、无重复，要找的 target 不一定在数组 nums 中。

target 在 nums 中没啥好说的，target 不在 nums 中，**无非就存在 3 种情况**：

- target < nums[0]，插在数组最前面
- target 插在某个数组元素后面
- target > nums[n-1]，插在数组最后面

基于这个情况，这道题的解题步骤如下：

- 初始化左右指针 low 和 high
- 每次计算中间指针 mid：若 num[mid] == target，直接返回 mid，若 nums[mid] < target，在右区间找，若 nums[mid] > target，在左区间找
- 查找结束，若没有相等的值，则返回 high + 1，此处为插入位置。

**说一下最后一步“若没有相等的值，则返回 high + 1”：**



> 对于二分查找，我的习惯查找的是［low, high］区间，区间是左闭右闭区间（当然你也可以是左闭右开区间）。
>
> 
>
> 此时的 while 循环的条件是 low <= high，那循环终止条件相当于是 low = high + 1，此时区间是 [high + 1, high]。
>
> 
>
> 循环终止代表 target 不在 nums 中，那它就要插入，插入的位置是 high 的右侧，即 high + 1（当然也可以是 left，毕竟此时 left 是等于 high + 1 的）。



如果还不理解的话找几个例子在纸上画一下，秒懂。

<div align=center>

![35-2](https://cdn.codegoudan.com/img/35-2.jpg)

</div>



# 图解

以 nums = [1,3,5,6], target = 0 为例。

首先初始化 low 和 high 指针。

<div align=center>

![35-3](https://cdn.codegoudan.com/img/35-3.png)

</div>

```Python
low, high = 0, len(nums) - 1
```

第 1 步，low = 0，high = 3，mid = (low + high) // 2 = 1：

<div align=center>

![35-4](https://cdn.codegoudan.com/img/35-4.png)

</div>

```Python
mid = (low + high) // 2
```

此时 nums[mid] = 3 > target，所以 high 向左移动至 mid - 1 = 0 处。

<div align=center>

![35-5](https://cdn.codegoudan.com/img/35-5.png)

</div>

```Python
if nums[mid] > target:
    high = mid - 1
```

第 2 步，low = 0，high = 0，mid = (low + high) // 2 = 0：

<div align=center>

![35-6](https://cdn.codegoudan.com/img/35-6.png)

</div>

此时 nums[mid] = 1 > target，所以 high 向左移动至 mid - 1 = -1 处。

<div align=center>

![35-7](https://cdn.codegoudan.com/img/35-7.png)

</div>

第 3 步，low = 0，high = -1，此时 high < low，while 循环中止。

此时没有在数组中找到 target，证明 target 小于数组中最小的元素，所以插在下标为 -1 的位置，成为新的下标 0，即 high + 1。

<div align=center>

![35-8](https://cdn.codegoudan.com/img/35-8.jpg)

</div>

本题解使用二分查找，**时间复杂度为 O(logn)**。

只额外定义了几个指针，**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:


        low, high = 0, len(nums) - 1

        # 在 [low, high] 找 target
        while low <= high:

            mid = (low + high) // 2

            # 如果 target 找到，直接返回位置
            if nums[mid] == target:
                return mid
            # 如果 target 大于中间数，则 target 可能在右区间
            # 在 [mid + 1, left] 中找
            elif nums[mid] < target:
                low = mid + 1
            # 如果 target 小于中间数，则 target 可能在左区间
            # 在 [left, mid -1] 中找
            else:
                high = mid - 1
        
        # 如果在数组中没找到，则返回需要插入数值的位置
        return high + 1
```



## Java 代码实现

```Java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0;
        int high = nums.length - 1;
        while (low <= high) { 
            int mid = low + (high - low) / 2; // 防止溢出
            if (nums[mid] > target) {
                high = mid - 1; 
            } else if (nums[mid] < target) {
                low = mid + 1; 
            } else {
                return mid;
            }
        }
        return high + 1;
    }
}
```



---

**图解搜索插入位置**到这就结束了。

还是要强调一下，**二分查找本身是没难度的，但是二分查找是个细节控，稍不注意就容易出错**。

大家在应用二分查找的时候还是要谨慎谨慎再谨慎。
