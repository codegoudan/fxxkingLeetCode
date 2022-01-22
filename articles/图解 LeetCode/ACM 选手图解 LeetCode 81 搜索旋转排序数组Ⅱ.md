**搜索旋转排序数组Ⅱ**，难度比【[搜索旋转排序数组](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924378&idx=1&sn=1d5cba8612e2d3ade15d9b93af0a419f&chksm=ff22fc17c85575019e82edef15f54e06e9eb0c33b6d829c569454dd5e98eadc1dc7dd359e605&scene=21#wechat_redirect)】做了升级，**数组中的元素不是唯一的，存在了重复元素**。

小样儿，套个马甲照样认识，话不多说，整它！

<div align=center>

![196b651f277f69c7c031b60f48bafae](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_172355061_0.jpg)

</div>



# LeetCode 81：搜索旋转排序数组Ⅱ

题目链接：[搜索旋转排序数组Ⅱ](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)



## 题意

整数数组 nums 非降序排列且可能存在重复元素。

给出从某个下标旋转后的数组 nums 和一个整数 target，如果 target 在 nums 中返回 true，否则返回 false。



## 示例

输入：nums = [2,5,6,0,0,1,2], target = 0

输出：true

解释：原 nums = [0,0,1,2,2,5,6]，从下标 4 旋转后变为现在的 nums = [2,5,6,0,0,1,2]。



## 提示

题目数据保证 nums 在预先未知的某个下标上进行了旋转。

- 1 <= len(nums) <= 5000
- -10^4 <= nums[i]、target <= 10^4



# 题目解析

二分查找经典好题，难度中等，是之前我们做过的【[**搜索旋转排序数组**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924378&idx=1&sn=1d5cba8612e2d3ade15d9b93af0a419f&chksm=ff22fc17c85575019e82edef15f54e06e9eb0c33b6d829c569454dd5e98eadc1dc7dd359e605&scene=21#wechat_redirect)】升级版。

搜索旋转排序数组中，整数数组 nums 原本是个升序排列且无重复元素的数组。

而在本题搜索旋转排序数组Ⅱ中，**整数数组 nums 成了非降序排列且可能出现重复元素的数组**。

升级在哪？

就升级在数组从【无重复元素】的数组变成了【可能存在重复元素】的数组。

【[**搜索旋转排序数组**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924378&idx=1&sn=1d5cba8612e2d3ade15d9b93af0a419f&chksm=ff22fc17c85575019e82edef15f54e06e9eb0c33b6d829c569454dd5e98eadc1dc7dd359e605&scene=21#wechat_redirect)】这道题我说过，**数组被旋转以后，总有一部分还是有序的**！

原先没重复元素的时候，比如 [4,5,6,7,0,1,2] 中，我找到 mid，不管是 0 ~ mid 或者 mid ~ n-1，总有半拉子是有序的。

要是 nums[low] <= nums[mid]，那就是左区间有序，否则右区间有序。

但是现在出现了重复元素以后，这样判断可能会失效。

因为可能有[4,5,4,4,4] 这种情况，此时 nums[low] == nums[mid] == nums[high]，这就没法判断左区间和右区间哪个是有序的。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_172802011_0.jpg" alt="48b2c6e4a585de8c15f47ade4d2ed15" style="zoom:50%;" />

</div>

碰到这种情况，一般的解决办法就是**收缩区间**。

**收缩区间就是 low 向右移动，同时 high 向左移动，直至出现 num[low] ！= nums[mid] 或者 nums[mid] ！= nums[high] 的情况**。

碰到不一样的，我们就可以用【[**搜索旋转排序数组**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924378&idx=1&sn=1d5cba8612e2d3ade15d9b93af0a419f&chksm=ff22fc17c85575019e82edef15f54e06e9eb0c33b6d829c569454dd5e98eadc1dc7dd359e605&scene=21#wechat_redirect)】的办法，判断左右区间哪个是有序的，哪个是无序的。

<div align=center>

![d8bb9fbf30bafe8bb557a4e671a1371](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_172818791_0.jpg)

</div>

具体步骤就成了：

- 找出 mid，如果 nums[mid] == target，直接返回。

- 如果 nums[low] == nums[mid] == nums[high]，low 向右移动，high 向左移动

- 如果 [low,mid-1] 有序：

- - target 在 [nums[low],nums[mid]] 中，范围缩小至 [low,mid-1]。
  - target 不在 [nums[low],nums[mid]] 中，范围缩小至 [mid+1,high]。

- 如果 [mid+1,high] 有序：

- - target 在 [nums[mid+1],nums[high]] 中，范围缩小至 [mid+1,high]。
  - target 不在 [nums[mid+1],nums[high]] 中，范围缩小至 [low,mid-1]。



# 图解

以 nums = [3,1,2,3,3,3,3], target = 2 为例。

首先初始化 low 和 high 指针。

<div align=center>

![8cbc0959b794b7f0d45ba4312ef2b21](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_172847785_0.jpg)

</div>

```Python
low, high = 0, len(nums) - 1
```

第 1 步，low = 0，high = 6，mid = low + (high - low) // 2 = 3：

<div align=center>

![7bca8f978cfe5a932add5d561b33115](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_172917313_0.jpg)

</div>

```Python
mid = low + (high - low) // 2
```

此时 nums[low] == nums[mid] == nums[high]，low 向右移动，high 向左移动。

<div align=center>

![a3f2c05b703cdb12c6d8c570c7558d4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_173707510_0.jpg)

</div>

```Python
# 相当于去重
if nums[low] == nums[mid] == nums[high]:
    low += 1
    high -= 1
```

第 2 步，low = 1，high = 5，mid = low + (high - low) // 2 = 3：

<div align=center>

![6d1c3a7dd0a80dcbdb767d5ecf81dc8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_173759215_0.jpg)

</div>

此时 nums[low] < nums[mid]，且 target 在左区间内，所以 high 向左移动至 mid - 1 = 2。

<div align=center>

![bc6150dcdc7c716c884bfb994bc1819](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_173826153_0.jpg)

</div>

```Python
# 如果左区间有序
elif nums[low] <= nums[mid]:
    # target 在左区间
    if nums[low] <= target < nums[mid]:
        high = mid - 1
```

第 3 步，low = 1，high = 2，mid = low + (high - low) // 2 = 1：

<div align=center>

![2d533a349344cc8fdcc7b23eb5eafdb](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_173900894_0.jpg)

</div>

此时 nums[low] <= nums[mid]，且 target 在右区间内，所以 low 向右移动至 mid + 1 = 2。

<div align=center>

![e41d6b2a7a71110ccfc0b50263fbc79](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_173915308_0.jpg)

</div>

第 3 步，low = 2，high = 2，mid = low + (high - low) // 2 = 2：

<div align=center>

![3695fa64f5b1e38adcb0d284b098577](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_173930743_0.jpg)

</div>

此时 nums[mid] == target，直接返回 True。

```Python
# 如果找到，返回结果
if nums[mid] == target:
    return True
```

本题解使用去重 + 二分查找的方式，因为存在 nums 数组中元素全部重复的情况，所以去重这一步的最坏时间复杂度为 O(n)，所以**总的时间复杂度为 O(n) + O(logn) = O(n)**。

同样是额外维护了几个指针，所以**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        if not nums:
            return False

        low, high = 0, len(nums) - 1

        while low <= high:
            mid = low + (high - low) // 2
            # 如果找到，返回结果
            if nums[mid] == target:
                return True
            # 相当于去重
            if nums[low] == nums[mid] == nums[high]:
                low += 1
                high -= 1
            # 如果左区间有序
            elif nums[low] <= nums[mid]:
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

        return False
```



## Java 代码实现

```Java
class Solution {
    public boolean search(int[] nums, int target) {
        int n = nums.length;
        if (n == 0) {
            return false;
        }
        if (n == 1) {
            return nums[0] == target;
        }
        int low = 0, high = n - 1;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (nums[mid] == target) {
                return true;
            }
            if (nums[low] == nums[mid] && nums[mid] == nums[high]) {
                ++low;
                --high;
            } else if (nums[low] <= nums[mid]) {
                if (nums[low] <= target && target < nums[mid]) {
                    high = mid - 1;
                } else {
                    low = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[high]) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }
        return false;
    }
}
```



---

**图解旋转排序数组Ⅱ**到这就结束辣，你看数组从元素唯一到元素存在重复，其实也就是加了一步区间收缩而已，其余的处理方式还是和之前【[**搜索旋转排序数组**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924378&idx=1&sn=1d5cba8612e2d3ade15d9b93af0a419f&chksm=ff22fc17c85575019e82edef15f54e06e9eb0c33b6d829c569454dd5e98eadc1dc7dd359e605&scene=21#wechat_redirect)】一样。

这就是我之前说过很多次的：**很多时候你就会发现，题目不过是在某类解决办法方面做加减法**。

当然这种感同身受更多的需要你自己在这个过程中去体验，大家加油！

