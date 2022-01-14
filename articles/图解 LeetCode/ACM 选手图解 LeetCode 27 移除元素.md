**移除元素**，是**快慢指针的经典题目**。

![10c1045ba37361eaa7aed40a7b96568](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181156126_0.jpg)



# LeetCode 27：移除元素

题目链接：[移除元素](https://leetcode-cn.com/problems/remove-element/)



## 题意

给你一个数组 nums 和一个 val，原地移除所有数值等于 val 的值，并返回移除后数组的新长度。



## 示例

输入：nums = [0,1,2,2,3,0,4,2], val = 2

输出：5, nums = [0,1,4,0,3]

解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。



## 提示

必须使用 O(1) 空间复杂度并原地修改输入数组。

- 0 <= len(nums) <= 100
- 0 <= nums[i] <= 50
- 0 <= val <= 100



# 题目解析

水题，题目简单。

这道题相当于数组元素的删除，既然涉及到数组的删除，就不是想当然的删除。

我在文章【[**ACM 选手带你玩转数组**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475918890&idx=1&sn=b5dbd85bbe358d7b1668e2d9c27e2122&chksm=ff22e1a7c85568b122279e836dec029dcc9b6a8049cf84c659ccff6f69eb29e6187912b1de66&scene=21#wechat_redirect)】中讲过：

数组是用**连续的一段内存空间，来存储相同数据类型**数据的集合。因为这个“连续内存存储”，**使得数组在插入或者删除的元素的时候，其它元素也要相应的移动。**

这道题数组的数据量很小，最多只到 100 个，所以可以用暴力手法破解。

所谓暴力手法就是最无脑的方式，两层 for 循环，第一层 for 循环从 0 开始遍历数组，第二层 for 循环维护后面的元素向前移动。

比如第一层 for 循环遍历到下标为 2 的位置发现 nums[2] == val，那么第二层循环就将 i+1 ~ n -1 位置上的元素全部向前移动一个位置。

因为是双重 for 循环，此时的时间复杂度为 O(n²)。

虽然也能 AC，但显然作为新时代的三好男人，本帅蛋在这方面追求的肯定不是慢，而是秒男的极致，我要快！

![531419fc699416d94ae936e8eec5a28](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181403277_0.jpg)

要我快点的话，这就不得不提**快慢指针**了。

**快慢指针，顾名思义，是使用速度不同的指针（可用在链表、数组、序列等上面），来解决一些问题。**

快慢指针用 fast 和 slow 两个指针控制，**快指针 fast 指向当前要和 val 对比的元素，慢指针 slow 指向将被赋值的位置**：

- 遍历数组。
- 如果 fast 指针指向的元素 nums[fast] != val，则 nums[fast] 是输出数组的元素，将其赋值到 nums[slow] 的位置，slow 和 fast 同时向后移动一位。
- 如果 nums[fast] == val，证明当前 nums[fast] 是要删除的元素，fast 向后移动一位。
- fast 遍历完整个数组，结束，slow 的值就是剩余数组的长度。



# 图解

以 nums = [0,1,2,2,3,0,4,2], val = 2 为例。

首先初始化 fast 和 slow 指针。

![cd1c475a49930e45c4bb51feb3a7271](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181510203_0.jpg)

```Python
# 初始化快慢指针
fast = slow = 0
```

遍历整个数组。

第 1 步，fast = 0，slow = 0，此时 nums[fast] = 0，不等于 val。

此时 nums[slow] = nums[fast]（虽然它俩现在本来就是一个），slow 和 fast 向后移动一位。

![25876de145355d74e629646b3e36e40](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181544348_0.jpg)

```Python
# 如果快指针指向的值不等于 val
# fast 指向位置的元素向 slow 指向的位置赋值
if nums[fast] != val:
    nums[slow] = nums[fast]
    slow += 1
    fast += 1
```

第 2 步，fast = 1，slow = 1，此时 nums[fast] = 1，不等于 val。

此时nums[slow] = nums[fast]，slow 和 fast 向后移动一位。

![4bc2813c314ec1ff1f15b55247ae5b3](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181616368_0.jpg)

第 3 步，fast = 2，slow = 2，此时 nums[fast] = 2，等于 val。

此时 fast 向后移动一位，slow 不动。

![8e3592db04761a2d481077f57ddba9e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181637893_0.jpg)

```Python
# 如果快指针指向的值等于 val
# 只 fast 指针移动，不向 slow 指针指向的位置赋值
fast += 1
```

第 4 步，fast = 3，slow = 2，此时 nums[fast] 依然是 2，等于 val。

所以还是 fast 向后移动一位，slow 不动。

![3198d260886f3edf211d0e3bb40ce02](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181716945_0.jpg)

第 5 步，fast = 4，slow = 2，此时 nums[fast] = 3，不等于 val。

此时nums[slow] = nums[fast] = 3，slow 和 fast 向后移动一位。

![10b0f075f12d8620ec842aa54f9d7be](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181733887_0.jpg)

第 6 步，fast = 5，slow = 3，nums[fast] = 0，不等于 val。

此时nums[slow] = nums[fast] = 0，slow 和 fast 向后移动一位。

![64b4fa5d11d5d992b60c840e8c80217](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181755237_0.jpg)

第 7 步，fast = 6，slow = 4，nums[fast] = 4，不等于 val。

此时nums[slow] = nums[fast] = 4，slow 和 fast 向后移动一位。

![358bcc605613b94bd6674c8681c4974](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_181818439_0.jpg)

第 8 步，fast = 7，slow = 5，nums[fast] = 2，和 val 相等。

此时 fast 向后移动一位，slow 不变，数组遍历结束。

可以看出，此时**从 [0, slow) 为剩下的输出数组，slow 的值为剩下数组的长度。**

本道题的解法，使用了一层 for 循环，所以**时间复杂度为 O(n)**。

额外使用了两个指针 fast 和 slow，**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:

        if len(nums) == 0:
            return 0
        # 初始化快慢指针
        fast = slow = 0

        while fast < len(nums):
            # 如果快指针指向的值不等于 val
            # fast 指向位置的元素向 slow 指向的位置赋值
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            
            # 如果快指针指向的值等于 val
            # 只 fast 指针移动，不向 slow 指针指向的位置赋值
            fast += 1

        return slow
```



## Java 代码实现

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        // 初始化快慢指针
        int fast = 0;
        int slow = 0;
        for (; fast < nums.length; fast++) {
            if (nums[fast] != val) {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;

    }
}
```



