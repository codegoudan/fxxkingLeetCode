<div align=center>

![153-3](https://cdn.codegoudan.com/img/153-3.png)

</div>

# LeetCode 153：寻找旋转排序数组中的最小值

题目链接：[搜索旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)



# 题意

已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次旋转后，得到输入数组。

例如：nums = [0,1,2,4,5,6,7] 旋转 4 次可以得到 [4,5,6,7,0,1,2]。

给你一个无重复元素的数组 nums，原来是一个升序数组，并按照上述情形做了多次旋转。

请你找出并返回数组中的最小元素。



## 示例

输入：[4,5,6,7,0,1,2]

输出：1

解释：原数组为 [0,1,2,4,5,6,7]，旋转 4 次得到输入数组。



## 提示

注意数组 [a[0],a[1],a[2],...,a[n-1]] 旋转一次的结果为数组 [a[n-1],a[0],a[1],...,a[n-2]]。

- 1 <= n == len(nums) <= 5000
- -5000 <= nums[i] <= 5000
- nums 中的所有整数互不相同
- nums 原先是一个升序数组，并进行了 1 ~ n 次旋转



# 题目解析

难度中等，值得一搞。

其实关于旋转排序数组，我们已经做过两种类型的了：



[**ACM 选手图解 LeetCode 搜索旋转排序数组**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924378&idx=1&sn=1d5cba8612e2d3ade15d9b93af0a419f&chksm=ff22fc17c85575019e82edef15f54e06e9eb0c33b6d829c569454dd5e98eadc1dc7dd359e605&scene=21#wechat_redirect)

[**ACM 选手图解 LeetCode 搜索旋转排序数组Ⅱ**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924710&idx=1&sn=9c05c9717b172d3b1763f4ebb974a228&chksm=ff22fb6bc855727d4c8db6895ef3974638b65632d764614b324cc69d57259208837ba6b05378&scene=21#wechat_redirect)



Ⅰ是数组 nums 无重复元素，Ⅱ是数组 nums 存在重复元素。

本题旋转排序数组和 Ⅰ 是一种类型，**数组 nums 无重复元素**。

但是本题又有不同，之前的题是给出一个 target，在 nums 中找是否存在 target，而本题是**直接在旋转数组中找最小值**。

题虽不同，但是做法没有大的改变，**无非就是维护一个最小值的指针，每次比较就更新当前的最小值**。

**如果你吃透了前面两道旋转排序数组，这道题对你来说，应该是简单题**。

具体步骤如下：

- 初始化一个最小值指针 nums_min。
- 找出 mid，如果 nums[mid] < nums_min，更新 nums_min。
- 如果左半区有序，维护 nums_min，范围缩小至[mid+1,high]。
- 否则就找右半区，维护 nums_min，范围缩小至[low,mid-1]。



# 图解

以 [4,5,6,7,0,1,2] 为例。

首先初始化 low，high 及 nums_min 指针。

<div align=center>

![153-4](https://cdn.codegoudan.com/img/153-4.png)

</div>

```Python
low, high = 0, len(nums) - 1
nums_min = float('inf')
```

第 1 步，low = 0，high = 6，mid = low + (high - low) // 2 = 3：

<div align=center>

![153-5](https://cdn.codegoudan.com/img/153-5.png)

</div>

```Python
mid = low + (high - low) // 2
```

此时 nums_min = inf， nums[mid] < nums_min，所以维护最小值指针 nums_min 的值。

<div align=center>

![153-6](https://cdn.codegoudan.com/img/153-6.png)

</div>

```Python
if nums[mid] < nums_min:
    nums_min = nums[mid]
```

当前 nums[low] < nums[mid]，证明左区间有序。

nums_min > nums[low]，维护最小值指针 nums_min，同时 low 向右移动至 mid + 1 处。

<div align=center>

![153-7](https://cdn.codegoudan.com/img/153-7.png)

</div>

```Python
if nums[low] < nums[mid]:
    if nums_min > nums[low]:
        nums_min = nums[low]
    low = mid + 1
```

第 2 步，low = 4，high = 6，mid = low + (high - low) // 2 = 5：

<div align=center>

![153-8](https://cdn.codegoudan.com/img/153-8.png)

</div>

此时 nums_min = 4， nums[mid] < nums_min，所以维护最小值指针 nums_min 的值。

<div align=center>

![153-9](https://cdn.codegoudan.com/img/153-9.png)

</div>

当前 nums[low] < nums[mid]，证明左区间有序。

nums_min > nums[low]，维护最小值指针 nums_min，同时 low 向右移动至 mid + 1 处。

<div align=center>

![153-10](https://cdn.codegoudan.com/img/153-10.png)

</div>

第 3 步，low = 6，high = 6，mid = low + (high - low) // 2 = 6：

<div align=center>

![153-11](https://cdn.codegoudan.com/img/153-11.png)

</div>

此时 nums_min = 0， nums[mid] > nums_min。

并且 nums[low] >= nums[mid]，证明右区间有序，nums_min < nums[high]，high 指针向右移动至 mid - 1。

<div align=center>

![153-12](https://cdn.codegoudan.com/img/153-12.png)

</div>

此时，high < low，循环结束，返回最小值 nums_min。

本题解使用二分查找，所以**时间复杂度为 O(logn)**。

只是额外的维护了几个指针，所以**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]

        low, high = 0, len(nums) - 1
        nums_min = float('inf')

        while low <= high:
            mid = low + (high - low) // 2

            if nums[mid] < nums_min:
                nums_min = nums[mid]

            if nums[low] < nums[mid]:
                if nums_min > nums[low]:
                    nums_min = nums[low]
                low = mid + 1

            else:
                if nums_min > nums[high]:
                    nums_min = nums[high]
                high = mid - 1

        return nums_min
```



## Java 代码实现

```Java
class Solution {
    public int findMin(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }

        int low = 0;
        int high = nums.length - 1;
        int nums_min = 5001;

        while (low <= high){
            int mid = low + (high - low) / 2;

            if(nums[mid] < nums_min){
                nums_min = nums[mid];
            }

            if(nums[low] < nums[mid]){
                if(nums_min > nums[low]){
                    nums_min = nums[low];
                }
                low = mid + 1;
            }else{
                if(nums_min > nums[high]){
                    nums_min = nums[high];
                }
                high = mid - 1;
            }
        }
        return nums_min;
    }
}
```


