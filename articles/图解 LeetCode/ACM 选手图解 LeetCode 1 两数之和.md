<div align=center>

![1-0](https://cdn.codegoudan.com/img/1-0.png)

</div>



# LeetCode 1：两数之和

题目链接：[两数之和](https://leetcode-cn.com/problems/two-sum/)



## 题意

找出 nums 数组中和为 target 的两个整数，并返回下标。



## 示例

输入：nums = [2,7,11,15], target = 9

输出：[0,1]

解释：

因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。



## 提示

- 2 <= nums.length <= 10^4
- -10^9 <= nums[i] <= 10^9
- -10^9 <= target <= 10^9
- **只会存在一个有效答案。**



# 题目解析

水题，难度简单。

这道题如果不考虑时间复杂度的话，最简单粗暴的方式就是暴力破解，枚举 nums 中的 x，再找数组中是否存在 target -x。

这种双重循环的时间复杂度是 O(n²)，这也太慢了，可以想一个时间复杂度小于 O(n) 的算法嘛？

答案肯定是可以的，用哈希可以高效解决。

为啥这么笃定呢？

就源于一句“找数组中是否存在 target -x”，**在一堆数中找一个数用哈希没跑儿**，毕竟作为查找中的秒男，哈希查找的时间复杂度是 O(1)。

<div align=center>

![1-1](https://cdn.codegoudan.com/img/1-1.jpg)

</div>

明白了这些，那两数之和的求解可以分为以下 3 步：

1. 创建一个哈希表。
2. 遍历 nums 数组，对于当前元素 nums[i]，查询哈希表中是否存在 target - nums[i]。
3. 若存在，返回下标，如不存在，nums[i] 存入哈希表，继续第 2 步。



# 图解

以 nums = [2,7,11,15]，target = 9 为例。

首先初始化一个哈希表，key 存储 nums[i]，value 存储 i。

```Python
# 初始化哈希表
hash = dict()
```

遍历数组 nums：

```Python
# 遍历数组 nums
for i in range(len(nums)):
```

第 1 步，i = 0，nums[0] = 2，此时哈希表为空，直接将 i = 0 和 nums[0] = 2 存入哈希表。

<div align=center>

![1-2](https://cdn.codegoudan.com/img/1-2.png)

</div>

```Python
# 若 target - nums[i] 不在哈希表中，将 nums[i] 存入哈希表
hash[nums[i]] = i
```

第 2 步，i = 1，nums[1] = 7，target = 9，9 - 7 = 2。

2 在哈希表中，对应的下标 value = 0，所以输出：[0,1]，算法结束。

<div align=center>

![1-3](https://cdn.codegoudan.com/img/1-3.png)

</div>

```Python
# 若 target - nums[i] 在哈希表中
if target - nums[i] in hash:
    # 返回结果
    return [hash[target - nums[i]], i]
```

本道题的解法，只遍历一遍数组，每个元素的查找时间复杂度为 O(1)，所以总的**时间复杂度为 O(n)**。

此外新建了一个哈希表，所以**空间复杂度为 O(n)**。

暴力破解本题的话，时间复杂度为 O(n²)，空间复杂度为 O(1)，对比两个方法，这就是我们平时所说的拿**空间换时间**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 初始化哈希表
        hash = dict()
        # 遍历数组 nums
        for i in range(len(nums)):
            # 若 target - nums[i] 在哈希表中
            if target - nums[i] in hash:
                # 返回结果
                return [hash[target - nums[i]], i]
            # 若 target - nums[i] 不在哈希表中，将 nums[i] 存入哈希表
            hash[nums[i]] = i

        return []
```



## Java 代码实现

```Java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        if (Objects.isNull(nums) || nums.length == 0) return null;
        //初始化哈希表 
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int curNum = target - nums[i];
            //是否存在 target - nums[i]的值
            if (map.containsKey(curNum)) {
                return new int[]{map.get(curNum), i};
            }
            //不存在 , 则进行存储
            map.put(nums[i], i);
        }
        return null;
    }
}
```



---

**图解两数之和**到这就结束辣，哈希学到现在小婊贝们是不是越来越有感觉了？

沉下心来，厚积薄发，总会在某个时间点，你发现自己突然变的不一样，切题如砍瓜切菜。

大家加油！
