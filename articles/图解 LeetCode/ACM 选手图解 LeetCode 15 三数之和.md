**三数之和**，这道题是面试中出现概率**非常非常高的高频题**。

对于这种类型的题一定要勤加练习，仔细揣摩。

<div align=center>

![2dd081fc1778452e9c083b83daa0e0e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_094752535_0.jpg)

</div>



# LeetCode 15：三数之和

题目链接：[三数之和](https://leetcode-cn.com/problems/3sum/)



## 题意

判断整数数组 nums 中是否存在三个元素 a、b、c，使得 a + b + c = 0。

要求：找出所有和为 0 且不重复的三元组。



## 示例

输入：nums = [-1,0,1,2,-1,-4]

输出：[[-1,-1,2],[-1,0,1]]



## 提示

- 0 <= nums.length <= 3000
- -10^5 <= nums[i] <= 10^5



# 题目解析

**超级好题，难度中等。**

这道题用哈希的解法，可以参考之前我给大家讲【两数之和】时的思路，忘记的可以回顾一下：



**[ACM 选手图解 LeetCode 之两数之和](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921946&idx=1&sn=73b27cbb317cb3572fc66a4a36e48181&chksm=ff22f597c8557c81f1ec21c30946d6b821fc749d8e90882089096267cdc95b54e2c965e2db71&scene=21#wechat_redirect)**



两数之和是在数组中找出 a + b = target，每次枚举 a，找数组中有没有 target - b，即公式可以变成 b = target - a。

同样三数之和也可以用这个思路，既然是在数组中找出三元素 a + b + c = 0，那我们可以每次枚举 a，b，然后去数组中查询有无 -(a + b) 即可，即公式可以变成 c = -(a + b)。

通过两次 for 循环枚举 a 和 b 的值，然后用哈希在 O(1) 的时间去在一堆数中查找 -（a + b）。

思路其实说起来挺简单的，但是说实话，**这道题用哈希来做的话，会把难度再提升一个 Level，不是在思路层面上，而是难在代码实现上。**

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_094930350_0.jpg" alt="9aaf7f5af1f676f6b0faa34dce13636" style="zoom:67%;" />

</div>

毕竟两重 for 循环的时间复杂度就已经到 O(n²) 了，而且这道题要求找出“不重复”的三元组，那就要求要考虑到去重这步操作。

**一些细节方面稍不注意，就容易 Time Limit Error（超时）。**

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_094952182_0.jpg" alt="f5f95008aba382e3dd26fe35f2a0db8" style="zoom:67%;" />

</div>



# 图解

以 nums = [-1,0,1,2,-1,-4] 为例。

在开始之前，先对 nums 进行排序，并初始化一个结果集合。

<div align=center>

![fca0a01a957248ec926f3f52722da55](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095058373_0.jpg)

</div>

```Python
# 对 nums 进行排序，这样在后面的时候可以减少重复带来的问题
nums.sort()
# res 存储最后的结果，set 集合的特点是无序且无重复
res = set()
```

之后就是双重 for 循环找 a，b。

第 1 步，i = 0，此时 a 即 nums[0] = -4，初始化哈希表 hash：

（1）j = 1，b 即 nums[1] = -1，此时 b = -1 不在哈希表中，将 -（a+b）= 5 加入哈希表中。

<div align=center>

![59b7c2f546c1879bec51182422f2a12](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095134703_0.jpg)

</div>

```Python
for i in range(len(nums)):
    # 初始化哈希表。
    hash = {}
    for j in range(i+1, len(nums)):
        # 如果不在哈希表中，加入哈希表
        if nums[j] not in hash:
            hash[-nums[i]-nums[j]] = 1
```

这里我想特别说明一下，**为什么把 -(a + b) 加到哈希表中，判断的是 b 不在哈希表中。**

这个也算是个小技巧吧。



> 你想，如果 b 在哈希表中，那就证明此时 b 的值等于之前某一个 - (a + b) 的值，那就证明存在三个数相加等于 0。
>
> 为了看不晕，我把此时的 b 叫做 b1，之前 - (a + b) 中的 a 还是 a，b 还是 b。
>
> 那公式其实就成了 a + b + b1 = 0，即 b1 = -(a + b)。
>
> 这里的 b1 就是我们意义上的 c。



这里一定要仔细揣摩，很重要。

（2）j = 2，b 即 nums[2] = -1，此时 b = -1 不在哈希表中，将 -（a+b）= 5 加入 哈希表中（因为在哈希表中，值就为 1，而不是累加，所以和上图一样）。

<div align=center>

![5fe8d44ad92e6da8c006d5333c8284e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095252191_0.jpg)

</div>

直到 j = 5，都未发现等于零的三元组，此时数组及哈希表如下：

<div align=center>

![18110a6500ed98bdf76415e6ab5f7f4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095343247_0.jpg)

</div>

第 2 步，i = 1，此时 a 即 nums[1] = -1，初始化哈希表 hash：

（1）j = 2，b 即 nums[2] = -1，此时 b = -1 不在哈希表中，将 -（a+b）= 2 加入哈希表中。

<div align=center>

![af66ed6cf472630fb70ea1b93d58d5e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095402422_0.jpg)

</div>

（2）j = 3，b 即 nums[3] = 0，此时 b = 0 不在哈希表中，所以将对应 -(a + b) = 1 加入哈希表中。

<div align=center>

![b01b2534b750578d1663e6a72efd6d9](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095419166_0.jpg)

</div>

（3）j = 4，b 即 nums[4] = 1，在哈希表中，此时的 a = nums[1] = -1，c = 此时的 b = 1，b = -(a + c) = 0。

```Python
# 如果在哈希表中，则存在三元组
else:
    res.add((nums[i],-nums[i]-nums[j],nums[j]))
```

（4） j = 5，b 即 nums[5] = 2，在哈希表中，此时的 a = nums[1] = -1，c = 此时的 b = 2，b = -（a + c）= 1。

第 3 步，i = 2，此时 a 即 nums[2] = -1，**此时 a 的值和 nums[1] 的值相同，直接跳过即可。**

为啥子呢？

<div align=center>

![18506991810a8cb4b41928f25265cfd](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095457828_0.jpg)

</div>

因为 a = nums[1] 的时候已经和后面的都玩过了，腻了，不需要再玩一遍（因为还是一样的结果）。

```Python
# 元素去重
if i > 0 and nums[i] == nums[i -1]:
    continue
```

第 4 步，接下来的步骤和上面是一样的，答案已经全部找完，后面大家自己动手试一下。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095530368_0.jpg" alt="94e64e64f665b251b62f97f4a5293bf" style="zoom:67%;" />

</div>



# 代码实现



## Python 代码实现

```Python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:

        # 如果 nums 个数 < 3，肯定组不成三元组
        if len(nums) < 3:
            return []
        # 对 nums 进行排序，这样在后面的时候可以减少重复带来的问题
        nums.sort()

        # res 存储最后的结果，set 集合的特点是无序且无重复
        res = set()

        for i in range(len(nums)):

            # 元素去重
            if i > 0 and nums[i] == nums[i -1]:
                continue

            # 初始化哈希表。
            hash = {}

            for j in range(i+1, len(nums)):

                # 如果不在哈希表中，加入哈希表
                if nums[j] not in hash:
                    hash[-nums[i]-nums[j]] = 1
                # 如果在哈希表中，则存在三元组
                else:
                    res.add((nums[i],-nums[i]-nums[j],nums[j]))

        return list(res)
```



## Java 代码实现

```Python
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                return result;
            }

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;
            while (right > left) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;
                    
                    right--; 
                    left++;
                }
            }
        }
        return result;
    }
}
```



---

**图解三数之和**到这就结束辣，是不是觉得难度有点不一样辣？

说点儿题外话，这道题其实最好的解法是“**排序 + 双指针**”，也比哈希法接起来简单且高效。

一题多解，用简单高效的方法解决掉这道题当然重要，但是我觉得更重要的是，**你在碰到一个问题的时候能够多想一步，一步一步再一步，不同维度不同姿势都尝试一下。**

刚开始这可能比较难，毕竟这涉及到一个改变，因为人都是有惰性的，毕竟只求一解比自找麻烦的求多解舒服多了...’

呃，希望小婊贝们能听进去，加油！

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220103_095846681_0.jpg" alt="f2d60ccbffc014ebd89bcd358cf5b86" style="zoom:67%;" />

</div>

