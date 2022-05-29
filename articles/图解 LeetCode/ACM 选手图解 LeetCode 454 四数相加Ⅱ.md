<div align=center>

![454-0](https://cdn.codegoudan.com/img/454-0.png)

</div>



# LeetCode 454：四数相加Ⅱ

题目链接：[四数相加Ⅱ](https://leetcode-cn.com/problems/4sum-ii/)



## 题意

给定 4 个长度为 n 的整数数组，计算有多少个元组(i, j, k, l) 能满足：

nums[i] + nums[j] + nums[k] + nums[l] == 0。



## 示例

输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]

输出：2

解释：

两个元组如下：

1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. 2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0



## 提示

- 1 <= n <= 200
- -2^28 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 2^28



# 题目解析

四数相加Ⅱ问题，难度中等。

这道题乍一看有点似曾相识，和我们上一道题【[**三数之和**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921996&idx=1&sn=1982f20d02cc44aa771b1710d3d28eae&chksm=ff22f5c1c8557cd7fd28ceaa995fb830613399b8e5b6f1f755f6166a37806a386a92585c52bb&scene=21#wechat_redirect)】有点差不多，但是架不住多看，一看原形毕露：因为实在是差的太多。

<div align=center>

![454-1](https://cdn.codegoudan.com/img/454-1.jpg)

</div>

我在【[**三数之和**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921996&idx=1&sn=1982f20d02cc44aa771b1710d3d28eae&chksm=ff22f5c1c8557cd7fd28ceaa995fb830613399b8e5b6f1f755f6166a37806a386a92585c52bb&scene=21#wechat_redirect)】中写过，它的最优解是“**排序 + 双指针**”，强行用哈希解是为了练习哈希，为了不超时需要对各种细节的处理将难度拔高了一个 level，纯粹是吃饱了撑的自己搞自己。

而**本道题则是哈希解法的亲儿子**，有 4 个独立的数组，只要找出 nums[i] + nums[j] + nums[k] + nums[l] = 0，同时**题目也没要求找出不重复的四元组，这就不需要考虑去重，在难度上降了不少**。

这道题知道了用哈希还不够，还需要做一下小处理。

不知大家还记不记得【[**两数之和**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921946&idx=1&sn=73b27cbb317cb3572fc66a4a36e48181&chksm=ff22f597c8557c81f1ec21c30946d6b821fc749d8e90882089096267cdc95b54e2c965e2db71&scene=21#wechat_redirect)】，遍历 nums 数组，对于当前元素 nums[i]，查询哈希表中是否存在 target - nums[i]。

四数相加Ⅱ的解法可以将四数分为两组，即“**分组 + 哈希**”：

- 初始化哈希表。
- 分组：nums1 和 nums2 一组，nums3 和 nums4 一组。
- 分别对 nums1 和 nums2 进行遍历，将所有 nums1 和 nums2 的值的和作为哈希表的 key，和的次数作为哈希表的 value。 
- 分别对 nums3 和 nums4 进行遍历，若 -(nums1[k] + nums4[l]) 在哈希表中，则四元组次数 +hash[-(nums3[k]+nums4[l])] 次。



# 图解

以 nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2] 为例。

首先初始化哈希表，**key 存储 nums[i] + nums[j] 的所有元素和，value 存储对应出现的次数**。

<div align=center>

![454-2](https://cdn.codegoudan.com/img/454-2.png)

</div>

```Python
# 初始化哈希表
hash = {}
cnt = 0
```

第一步，分别对 nums1 和 nums2 中的元素进行遍历，将所有 nums1 和 nums2 的值的和作为哈希表的 key，和的次数作为哈希表的 value。

nums1 的第 1 个元素为 1，nums2 的第 1 个元素为 -2，1 + (-2) = -1，-1 不在哈希表中，遂加入哈希表。

<div align=center>

![454-3](https://cdn.codegoudan.com/img/454-3.png)

</div>

```Python
# 首先存储前两个数组之和
for n1 in nums1:
    for n2 in nums2:
        # 如果在哈希表中，则对应哈希值 +1
        if n1 + n2 in hash:
            hash[n1 + n2] += 1
        # 如果不在哈希表中，放入哈希表
        else:
            hash[n1 + n2] = 1
```

遍历完 nums1 和 nums2 所有元素，哈希表变为如下所示：

<div align=center>

![454-4](https://cdn.codegoudan.com/img/454-4.png)

</div>

即 hash[-1] = 1，hash[0] = 2，hash[1] = 1。

第二步，分别对 nums3 和 nums4 进行遍历，若 -(nums1[k] + nums4[l]) 在哈希表中，则四元组次数 +hash[-(nums3[k]+nums4[l])] 次。

遍历 nums3 和 nums4 中所有元素，nums3 的第 1 个元素为 -1，nums4 的第 1 个元素为 0， - (-1 + 0) = 1，1 在哈希表中，并且 1 的 value 为 1。

所以此时存在四元组(i, j, k, l) = (1, 1, 0, 0)，相加为 0，cnt = 1。

<div align=center>

![454-5](https://cdn.codegoudan.com/img/454-5.png)

</div>

遍历完 nums3 和 nums4 的所有元素，找到 2 个四元组 (1, 1, 0, 0) 和 (0, 0, 0, 1)，此时 cnt = 2。

```Python
# 统计剩余两个数组的和，在哈希表中找是否存在相加为 0 的情况。
for n3 in nums3:
    for n4 in nums4:
        if -(n3 + n4) in hash:
            cnt += hash[-(n3 + n4)]
```

本题解法用了两次双重循环，**时间复杂度为 O(n²)**。

同时额外维护了一个哈希表，最坏情况下 nums1[i] + nums[j] 的值全不相同，所以**空间复杂度为 O(n²)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:

        # 初始化哈希表
        hash = {}
        cnt = 0

        # 首先存储前两个数组之和
        for n1 in nums1:
            for n2 in nums2:
                # 如果在哈希表中，则对应哈希值 +1
                if n1 + n2 in hash:
                    hash[n1 + n2] += 1
                # 如果不在哈希表中，放入哈希表
                else:
                    hash[n1 + n2] = 1
        # 统计剩余两个数组的和，在哈希表中找是否存在相加为 0 的情况。
        for n3 in nums3:
            for n4 in nums4:
                if -(n3 + n4) in hash:
                    cnt += hash[-(n3 + n4)]

        return cnt
```



## Java 代码实现

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> map = new HashMap<>();
        int temp;
        int res = 0;
        //统计两个数组中的元素之和，同时统计出现的次数，放入map
        for (int i : nums1) {
            for (int j : nums2) {
                temp = i + j;
                if (map.containsKey(temp)) {
                    map.put(temp, map.get(temp) + 1);
                } else {
                    map.put(temp, 1);
                }
            }
        }
        //统计剩余的两个元素的和，在map中找是否存在相加为0的情况，同时记录次数
        for (int i : nums3) {
            for (int j : nums4) {
                temp = i + j;
                if (map.containsKey(0 - temp)) {
                    res += map.get(0 - temp);
                }
            }
        }
        return res;
    }
}
```


