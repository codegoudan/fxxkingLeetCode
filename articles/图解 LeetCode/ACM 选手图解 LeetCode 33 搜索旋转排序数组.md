大家好呀，我是旋转蛋。

今天解决**搜索旋转排序数组**，用二分查找解决**局部有序**数组的经典问题。

话不多说，让我们来会一会它。

<div align=center>

![33-0](https://cdn.codegoudan.com/img/33-0.png)

</div>



# LeetCode 33：搜索旋转排序数组

题目链接：[搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)



## 题意

整数数组 nums 升序排列且无重复元素。

给你从某个下标旋转后的数组 nums 和 target，如果 target 在 nums 中返回下标，否则返回 -1。



## 示例

输入：nums = [4,5,6,7,0,1,2], target = 0

输出：4

解释：原 nums = [0,1,2,4,5,6,7]，从下标 3 旋转后变为现在的 nums = [4,5,6,7,0,1,2]



## 提示

数据保证 nums 在预先未知的某个下标上进行了旋转，nums 中的每个值独一无二。

- 1 <= len(nums) <= 5000
- -10^4 <= nums[i]、target <= 10^4



# 题目解析

又是一种新的题型，难度中等。

我们早就知道，二分查找必须的条件是数组有序。

刚看这题题意第一行的时候，整数数组 nums 升序排列且无重复元素，当时我就默默的掏出了我的二分查找板子。

辣鸡，写的这么明显，当我是傻子嘛。

<div align=center>

![33-1](https://cdn.codegoudan.com/img/33-1.jpg)

</div>

然后还没开心到高潮，发现 nums 被人扭了一下，旋转了。

小丑竟是我自己...

出题人你出来，你当数组是尼玛魔方嘛，想扭就扭。

<div align=center>

![33-2](https://cdn.codegoudan.com/img/33-2.jpg)

</div>

不过再定睛一看，这题二分查找还是可解。

因为**数组被旋转以后，总有一部分是有序的**！

比如 [4,5,6,7,0,1,2] 中，我找到 mid，不管是 0 ~ mid 或者 mid ~ n-1，总有半拉子是有序的，我们在有序的那边进行二分查找是可以的。

那这样**二分查找稍微变一下，加个有序的判断**，这道题的解法就出来了：

- 找出 mid，如果 nums[mid] == target，直接返回。

- 如果 [low,mid-1] 有序：

- - target 在 [nums[low],nums[mid]] 中，范围缩小至 [low,mid-1]。
  - target 不在 [nums[low],nums[mid]] 中，范围缩小至 [mid+1,high]。

- 如果 [mid+1,high] 有序：

- - target 在 [nums[mid+1],nums[high]] 中，范围缩小至 [mid+1,high]。
  - target 不在 [nums[mid+1],nums[high]] 中，范围缩小至 [low,mid-1]。

下面我们来看图解。



# 图解

以 nums = [4,5,6,7,0,1,2], target = 0 为例。

首先初始化 low 和 high 指针。

<div align=center>

![33-3](https://cdn.codegoudan.com/img/33-3.png)

</div>

```Python
low, high = 0, len(nums) - 1
```

第 1 步，low = 0，high = 6，mid = low + (high - low) // 2 = 3：

<div align=center>

![33-4](https://cdn.codegoudan.com/img/33-4.png)

</div>

```Python
mid = low + (high - low) // 2
```

此时 nums[low] < nums[mid]，且 target 不在左区间内，所以 low 向右移动至 mid + 1 = 4。

<div align=center>

![33-5](https://cdn.codegoudan.com/img/33-5.png)

</div>

```Python
# 如果左区间有序
if nums[low] <= nums[mid]:
    # target 在左区间
    if nums[low] <= target < nums[mid]:
        high = mid - 1
    # target 在右区间
    else:
        low = mid + 1
```

第 2 步，low = 4，high = 6，mid = low + (high - low) // 2 = 5：

<div align=center>

![33-6](https://cdn.codegoudan.com/img/33-6.png)

</div>

此时 nums[low] < nums[mid]，且 target 在左区间内，所以 high 向左移动至 mid - 1 = 4。

<div align=center>

![33-7](https://cdn.codegoudan.com/img/33-7.png)

</div>

第 3 步，low = 4，high = 4，mid = low + (high - low) // 2 = 4：

<div align=center>

![33-8](https://cdn.codegoudan.com/img/33-8.png)

</div>

此时 nums[mid] == target ，直接返回结果。

```Python
# 如果找到，返回结果
if nums[mid] == target:
    return mid
```

本题解使用二分查找，**时间复杂度为 O(n)**。

同时只额外维护了几个指针，所以**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if not nums:
            return -1

        low, high = 0, len(nums) - 1

        while low <= high:
            mid = low + (high - low) // 2
            # 如果找到，返回结果
            if nums[mid] == target:
                return mid
            # 如果左区间有序
            if nums[low] <= nums[mid]:
                # target 在左区间
                if nums[low] <= target < nums[mid]:
                    high = mid - 1
                # target 在右区间
                else:
                    low = mid + 1
            # 如果右区间有序
            else:
                # target 在右区间
                if nums[mid] < target <= nums[high]:
                    low = mid + 1
                # target 在左区间
                else:
                    high = mid - 1

        return -1
```



## Java 代码实现

```Java
class Solution {
    public int search(int[] nums, int target) {
        int n = nums.length;
        if (n == 0) {
            return -1;
        }
        if (n == 1) {
            return nums[0] == target ? 0 : -1;
        }
        int l = 0, r = n - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[0] <= nums[mid]) {
                if (nums[0] <= target && target < nums[mid]) {
                    r = mid - 1;
                } 
                else {
                    l = mid + 1;
                }
            } 
            else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } 
                else {
                    r = mid - 1;
                }
            }
        }
        return -1;
    }
}
```
