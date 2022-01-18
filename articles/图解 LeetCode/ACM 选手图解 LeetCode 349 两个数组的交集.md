<div align=center>

![1f9eb3bd41efca694d22e8ee3b460e6](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_222340324_0.jpg)

</div>

# LeetCode 349：两个数组的交集

题目链接：[两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)



## 题意

给定两个数组，计算它们的交集。



## 示例

输入：nums1 = [4, 9, 5]，nums2 = [9, 4, 9, 8, 4]

输出：[９, 4]



## 提示

- 输出结果中的每个元素一定是唯一的。
- 我们可以不考虑输出结果的顺序。



# 题目解析

水题，难度简单。

就一句话：求两个数组的交集，直白点儿就是【nums2 的元素是否在 nums1 中】。

需要注意一下“提示”里讲的：输出结果中的每个元素一定是唯一的，翻译一下就是去重输出。

这道题为啥能用哈希做呢？

我在之前【[**LeetCode202 快乐数**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921749&idx=1&sn=14793c51448317a6c9dfb4e263a9cc90&chksm=ff22f6d8c8557fce36547a97d745c21c5c941d2777a118ec760fe771832071d90222316d3d89&scene=21#wechat_redirect)】中讲过：**在一堆数中查找一个数，当然是扔出哈希**，毕竟是查找中的秒男，时间复杂度为 O(1)。

这道题和快乐数有些相似的地方：这道题同样也没限制数值的大小，所以就不能用数组来做哈希表。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_222525438_0.jpg" alt="a0ad6fbe1ddbe6bf2c361bad5e81c90" style="zoom:67%;" />

</div>

**碰到这种对目前来说是未知数值大小的情况，我们可以使用集合 set 来解决。**

用 set 爽归爽，在这里我建议还是不要直接用库函数。

大家用的编程语言可能不尽相同，**最好是用编程语言自带的数据结构来模拟一下**，毕竟对于初学者来说，还是得脚踏实地一步步的搞。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_222552008_0.jpg" alt="c5acb7ae66ae56b88131bddd0ee803a" style="zoom: 67%;" />

</div>



# 图解

以 nums1 = [4, 9, 5]，nums2 = [9, 4, 9, 8, 4] 为例。

首先初始化一个哈希表和一个结果列表。

```Python
# 初始化哈希表
hash = {}
# 初始化结果列表，存放最后结果
res = []
```

第一步，遍历数组 nums1，将出现的数存进哈希表中。

nums1 数组中，第 1 个元素为 4，将其加入哈希表，对应数值置为 1。

<div align=center>

![9e70a9a3d0de652c9aa958f24ecba5f](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_222659426_0.jpg)

</div>

```Python
# 哈希表 key 为 nums1 数组中的数，value 为值
for i in nums1:
    if not hash.get(i):
        hash[i] = 1
```

因为【输出结果中的每个元素一定是唯一的】，所以对于 key 所对应的 value 来说“数值是多少”就无所谓了，所以在本题中，不管某个元素在数组中出现多少次，我把 value 都置为 1。

遍历完数组 nums1，哈希表如下图所示：

<div align=center>

![0fd9ab3cfe7af40b4351793c1e2d0ef](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_222735296_0.jpg)

</div>

第二步，遍历 nums2 数组，nums2 数组中的元素如果出现在哈希表中，则证明是和 nums1 数组相交的元素，则加入结果列表中。

nums2 数组中，第 1 个元素为 9，9 在哈希表中，则元素 9 加入结果列表，哈希表中该元素置为 0。

<div align=center>

![c7cda79de23562a80f9250a74b5d5fd](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_222751915_0.jpg)

</div>

```Python
# 遍历 nums，如果 nums2 数组中的数出现在哈希表中，对应数放入结果列表，对应 value 值置-为0
for j in nums2:
    if hash.get(j):
        res.append(j)
        hash[j] = 0
```

同样遍历完 nums2，最后的结果变为下图所示：

<div align=center>

![adc1f9857f3351b400db26b92e52466](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_222819096_0.jpg)

</div>

res 就是最终的结果。

本题解遍历了 nums1 和 nums2 数组，所以**时间复杂度为 O(n + m)**，n 和 m 分别为两个数组的长度。

额外建了一个哈希表，所以**空间复杂度为 O(max(n, m))**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

        # 有一个数组为空，则交集为空
        if not nums1 or not nums2:
            return []

        # 初始化哈希表
        hash = {}
        # 初始化结果列表，存放最后结果
        res = []

        # 哈希表 key 为 nums1 数组中的数，value 为值
        for i in nums1:
            if not hash.get(i):
                hash[i] = 1
        # 遍历 nums，如果 nums2 数组中的数出现在哈希表中，对应数放入结果列表，对应 value 值置-为0
        for j in nums2:
            if hash.get(j):
                res.append(j)
                hash[j] = 0

        return res
```



## Java 代码实现

```Java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) {
            return new int[0];
        }
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> resSet = new HashSet<>();
        //遍历 nums1
        for (int i : nums1) {
            set1.add(i);
        }
        //遍历nums2，判断哈希表中是否存在该元素
        for (int i : nums2) {
            if (set1.contains(i)) {
                resSet.add(i);
            }
        }
        int[] resArr = new int[resSet.size()];
        int index = 0;
        for (int i : resSet) {
            resArr[index++] = i;
        }
        return resArr;
    }
}
```



---

**图解两个数组的交集**到这就结束辣。

这是哈希实战的第 4 题，不知道大家有没有发现我的小心思：

第 1 题【[**LeetCode242 有效的字母异位词**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921505&idx=1&sn=edf8b3420e0b1bf406f98a990074bbe0&chksm=ff22f7ecc8557efa3edfac0d6f107b67bffca87d5debb474e1d33b7b12a7c73ba3b756a280d1&scene=21#wechat_redirect)】和第 2 题【[**LeetCode383 赎金信**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921671&idx=1&sn=dbbdcd6ee3704e7d64d00c015f66951f&chksm=ff22f68ac8557f9ceac327e81b5a1b70b3b8f9fa82e6497329b97c5aaef7f32f5fa943df59dc&scene=21#wechat_redirect)】，第 3 题【[**LeetCode202 快乐数**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921749&idx=1&sn=14793c51448317a6c9dfb4e263a9cc90&chksm=ff22f6d8c8557fce36547a97d745c21c5c941d2777a118ec760fe771832071d90222316d3d89&scene=21#wechat_redirect)】和两个数组的交集是一类题。

**相同类型的题刷多了，你就会发现，最后其实在你脑子里记住的不是实现这道题的代码，而是解这道题的思路。**

等什么时候出现类似“**这道题我见过，我知道用这样哪样的方法可以做出来**”，小婊贝们就快出师了。
