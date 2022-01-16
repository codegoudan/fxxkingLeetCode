**长度最小的子数组**，滑动窗口解决的经典题目，之前做过一道用队列求[**滑动窗口最大值**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475920459&idx=1&sn=3f7a5d2c9d01d2d0b7e7aba0cf3140d7&chksm=ff22ebc6c85562d0726fbfeccf6e987b4d5b379e50942c981e7075a710eca10af38806833047&scene=21#wechat_redirect)。

滑动窗口呢，一般就用在数组或者字符串上，我们先从字面上来认识一下滑动窗口：

- **滑动**：窗口可以按照一定的方向移动。
- **窗口**：窗口大小可以固定，也可以不固定，此时可以向外或者向内，扩容或者缩小窗口直至满足条件。

<div align=center>

![0583948380e072181bbb06d8ae165d3](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_183955023_0.jpg)

</div>

# LeetCode 209：长度最小的子数组

题目链接：[长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)



## 题意

给定含有 n 个正整数的数组和一个正整数 target：

找出该数组中满足其和 ≥ target 的长度最小的连续子数组，并返回其长度，若不存在，返回 0。



## 示例

输入：target = 7, nums = [2,3,1,2,4,3]

输出：2

解释：子数组 [4,3] 是该条件下的长度最小的子数组。



## 提示

- 1 <= target <= 10^9
- 1 <= len(nums) <= 10^5
- 1 <= nums[i] <= 10^5



# 题目解析

**中等难度题，但是知道了思路，代码难度几乎没有，属于中等难度中的菜鸡**。

通常最开始想到的最简单的方法肯定是暴力查找。

遍历数组 nums，每次将一个下标的元素作为子数组的开始，接下来从下标 i 开始向后遍历，找到最小下标 j，使得从 nums[i] 到 nums[j] 的和 >= target，更新子数组的最小长度。

这种方式使用双重 for 循环，时间复杂度为 O(n²)，鉴于 nums 的最大长度为 10^5，显然不是好办法。

不过我无聊试了一下，在 LeetCode 上暴力法是可以 AC 的，不过执行时间看的我直嘬牙花子。

<div align=center>

![0fb63ea4d6407eec109f55ec878ecbf](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185022237_0.jpg)

</div>

而这道题比较经典的解法就是**滑动窗口**。

滑动窗口，看起来名字挺高大上，有点唬人，但是禁不住细看。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185049124_0.jpg" alt="158ac9cf092989a1a2c22262ae2d10b" style="zoom:67%;" />

</div>

在文章开头我大概介绍了滑动窗口是啥，这本质上和解决【[**LeetCode 29 有序数组的平方**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475922258&idx=1&sn=95bfe3da6211191d5335cb520820bc99&chksm=ff22f4dfc8557dc9eca2d93ed8b28e4e01c8557ed7e239b6b0265ac7d13ea50d44ab310637c2&scene=21#wechat_redirect)】里用的 left 和 right 双指针没啥区别，只不过动的更自如，left 和 right 之间的间隔可以忽大忽小。

知道了这些，那解题步骤就很明确了：

- 使用左右指针 left 和 right，left 和 right 之间的长度即为滑动窗口的大小（即连续数组的大小）。
- 如果滑动窗口内的值 sum >= target，维护连续数组最短长度，left 向右移动，缩小滑动窗口。
- 如果滑动窗口内的值 sum < target，则 right 向有移动，扩大滑动窗口。

楞看可能还是有点懵，下面我们来看图解。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185110052_0.jpg" alt="963f4b22b0222827851234abbacf49c" style="zoom:67%;" />

</div>



# 图解

以 target = 7, nums = [2,3,1,2,4,3] 为例。

首先初始化 left 和 right 指针以及当前和sum、最小长度 min_len。

<div align=center>

![7ab790ebded33242092971140ab6508](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185132385_0.jpg)

</div>

```Python
# 滑动窗口左右边界
left = right = 0
# 记录当前元素和
sum = 0
# 记录最短长度
min_len = float('inf')
```

第 1 步：

- left = 0，right = 0，target = 7
- sum = sum + nums[right] = 0 + 2 = 2
- sum < target
- right 指针向右移动，扩大窗口

<div align=center>

![61046488ceca1d3012c3cd649d843e1](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185210781_0.jpg)

</div>

```Python
sum += nums[right]
while sum < s:
     right += 1
```

第 2 步：

- left = 0，right = 1，target = 7
- sum = sum + nums[right] = 2 + 3 = 5
- sum < target
- right 指针向右移动，扩大窗口

<div align=center>

![5a21a2b1f3cf7188da5f715cf160671](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185250494_0.jpg)

</div>

第 3 步：

- left = 0，right = 2，target = 7
- sum = sum + nums[right] = 5 + 1 = 6
- sum < target
- right 指针向右移动，扩大窗口

<div align=center>

![cc83cf6c55cd8493bf59616dc779a55](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185413614_0.jpg)

</div>

第 4 步：

- left = 0，right = 3，target = 7
- sum = sum + nums[right] = 6 + 2 = 8
- sum > target
- min_len = min(min_len, right - left +1) = min(inf, 4) = 4
- sum = sum - nums[left] = 8 - 2 = 6
- left 指针向右移动，缩小窗口

<div align=center>

![8c23e7c2c8f2dc4374d661f44b3a4e2](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185433281_0.jpg)

</div>

```Python
while sum >= s:
    # 取之前窗口长度和当前窗口长度最短的
    min_len = min(min_len, right - left + 1)
    # 去掉左侧的数
    sum -= nums[left]
    # 缩小窗口
    left += 1
```

第 5 步：

- left = 1，right = 3，target = 7，sum = 6
- sum < target
- right 指针向右移动，扩大窗口

<div align=center>

![15ad4a91e70d38361a0f6c2f30579d8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185509683_0.jpg)

</div>

第 6 步：

- left = 1，right = 4，target = 7
- sum = sum + nums[right] = 6 + 4 = 10
- sum > target
- min_len = min(min_len, right - left +1) = min(4, 4) = 4
- sum = sum - nums[left] = 10 - 3 = 7
- left 指针向右移动，缩小窗口

<div align=center>

![711a41105eb8216f254d9a22ec9f5d0](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185528373_0.jpg)

</div>

第 7 步：

- left = 2，right = 4，target = 7，sum = 7
- sum >= target
- min_len = min(min_len, right - left +1) = min(4, 3) = 3
- sum = sum - nums[left] = 7 - 1 = 6
- left 指针向右移动，缩小窗口

<div align=center>

![49a3705c3bd963aa79a3a03566b823a](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185554396_0.jpg)

</div>

第 8 步：

- left = 3，right = 4，target = 7，sum = 6
- sum < target
- right 指针向右移动，扩大窗口

<div align=center>

![ed92a3cfdb6f0fc5c63216450274e4e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185610388_0.jpg)

</div>

第 9 步：

- left = 3，right = 5，target = 7
- sum = sum + nums[right] = 6 + 3 = 9
- sum > target
- min_len = min(min_len, right - left +1) = min(3, 3) = 3
- sum = sum - nums[left] = 9 - 2 = 7
- left 指针向右移动，缩小窗口

<div align=center>

![54c3bf055e8ce0d1a089ef21e06f485](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185632672_0.jpg)

</div>

第 10 步：

- left = 4，right = 5，target = 7，sum = 7
- sum >= target
- min_len = min(min_len, right - left +1) = min(3, 2) = 2
- sum = sum - nums[left] = 7 - 4 = 3
- right 已遍历完数组，end

<div align=center>

![ca62190d7f89c600c732efc2007048e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_185651974_0.jpg)

</div>

本题解 left 指针和 right 指针最多只遍历数组 1 次，所以**时间复杂度为 O(n)**。

只额外开了 2 个指针，**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:

        if not nums:
            return 0

        n = len(nums)
        # 滑动窗口左右边界
        left = right = 0
        # 记录当前元素和
        sum = 0
        # 记录最短长度
        min_len = float('inf')

        while right < n:

            sum += nums[right]
            # 如果当前元素和 >= s
            while sum >= s:
                # 取之前窗口长度和当前窗口长度最短的
                min_len = min(min_len, right - left + 1)
                # 去掉左侧的数
                sum -= nums[left]
                # 缩小窗口
                left += 1
            right += 1

        # 如果整个数组所有元素的和相加都 < s
        # 即不存在符合条件的子数组，返回 0
        if min_len == float('inf'):
            return 0
        else:
            return min_len
```



## Java 代码实现

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int left = 0;
        int sum = 0;
        int result = Integer.MAX_VALUE;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum >= s) {
                result = Math.min(result, right - left + 1);
                sum -= nums[left++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```



