多数元素，题目的定义是指在数组中出现次数大于 n/2 的元素。

说白了就是求数组的**众数**。

众数是一个数学名词，常用在统计学中，是一组数据中出现次数最多的数值，代表数据的一般水平。

下面让我们来搞一下这道题。

<div align=center>

![169-1](https://cdn.codegoudan.com/img/169-1.png)

</div>



# LeetCode 169：多数元素

题目链接：[多数元素](https://leetcode-cn.com/problems/majority-element/)



## 题意

给定一个大小为 n 的数组，找到其中的多数元素。

多数元素是指在数组中出现次数大于 n/2 的元素。



## 示例

输入：[2,2,1,1,1,2,2]

输出：2



## 提示

- 数组非空，且给定的数组总是存在多数元素。



# 题目解析

本题求多数元素，难度简单。

这道题解法众多，既然准备用分治算法来解决，还是按照分治求解的三步走：

- 划分（Divide）
- 求解（Conquer）
- 合并（Combine）

那整个问题的解决步骤就很明确了：

**(1) 划分**

划分就是拆解到问题的最小规模，这里还是用到了二分的思想。

每次将数组拆分为左右两个区间，直至拆成最小规模的问题，每个区间只有一个数。

**(2) 求解**

递归的求解划分之后的子问题。

在最小的区间里，每个区间只有一个数，那该区间的众数该数。

**(3) 合并**

一步步的向上合并，合并过程中分为两种情况：

第一种：左右两个区间的众数相同，那直接返回这个众数。

第二种：左右两个区间的众数不同，这时就将两个区间合并，在合并后的区间种计算出这两个众数出现的次数，将数值大的众数返回。

这里面要注意一点的是，**可能出现左右两个众数在合并后的区间中出现的次数相同，这种情况就先随便返回一个即可**。

因为这只是过程中的产生情况，在合并的最终解不会出现这种情况。

毕竟【提示】中说了：数组非空，且给定的数组总是存在多数元素。



# 图解

以 nums = [2,2,,1,1,1,2,2] 为例。

第 1 步，划分：

<div align=center>

![169-2](https://cdn.codegoudan.com/img/169-2.png)

</div>

```Python
# 递归划分左右区间
mid = left + (right - left) // 2
maxLeft = self.getMajority(nums, left, mid)
maxRight = self.getMajority(nums, mid + 1, right)
```

第 2 步，求解：

<div align=center>

![169-3](https://cdn.codegoudan.com/img/169-3.png)

</div>

```Python
if left == right:
    return nums[left]
```

第 3 步，合并：

<div align=center>

![169-4](https://cdn.codegoudan.com/img/169-4.png)

</div>

```Python
if maxLeft == maxRight:
    return maxLeft

# 如果左右众数不相等
else:
    leftCnt, rightCnt = 0, 0
    
    # 合并区间找众数
    for i in range(left, right + 1):
        if maxLeft == nums[i]:
            leftCnt += 1

        if maxRight ==  nums[i]:
            rightCnt += 1

    if leftCnt >= rightCnt:
        return maxLeft
    else:
        return maxRight
```

本题解不断将数组分为左右区间，这个时间复杂度是 O(logn)，在向上合并解的时候，在左右众数不相等的时候，需要遍历合并后的区间，判断众数 leftCnt 和 rightCnt 的大小，这个的时间复杂度为 O(n)，所以总的**时间复杂度为 O(nlogn)**。

分治算法没有直接分配额外的数组空间，但是在递归过程中调用了额外的栈空间，分治算法相当于每次将当前问题拆成了两部分，n 在变为 1 之前需要进行 O(logn) 次递归，所以**空间复杂度为 O(logn)**。



# 代码实现



## Python 代码实现

```Python
class Solution:

    def getMajority(self, nums:List[int], left, right):

        if left == right:
            return nums[left]

        # 递归划分左右区间
        mid = left + (right - left) // 2
        maxLeft = self.getMajority(nums, left, mid)
        maxRight = self.getMajority(nums, mid + 1, right)

        # 如果左边众数 = 右边的众数
        if maxLeft == maxRight:
            return maxLeft

        # 如果左右众数不相等
        else:
            leftCnt, rightCnt = 0, 0
            
            # 合并区间找众数
            for i in range(left, right + 1):
                if maxLeft == nums[i]:
                    leftCnt += 1

                if maxRight ==  nums[i]:
                    rightCnt += 1

            if leftCnt >= rightCnt:
                return maxLeft
            else:
                return maxRight

    def majorityElement(self, nums: List[int]) -> int:

        return self.getMajority(nums, 0, len(nums) - 1)
```



## Java 代码实现

```Java
class Solution {
    private int getCnt(int[] nums, int num, int left, int right) {
        int cnt = 0;
        for (int i = left; i <= right; i++) {
            if (nums[i] == num) {
                cnt++;
            }
        }
        return cnt;
    }

    private int getMajority(int[] nums, int left, int right) {
        if (left == right) {
            return nums[left];
        }

        int mid = left + (right - left) / 2;
        int maxLeft = getMajority(nums, left, mid);
        int maxRight = getMajority(nums, mid + 1, right);

        if (maxLeft == maxRight) {
            return maxLeft;
        }

        int leftCnt = getCnt(nums, maxLeft, left, right);
        int rightCnt = getCnt(nums, maxRight, left, right);

        return leftCnt >= rightCnt ? maxLeft : maxRight;
    }

    public int majorityElement(int[] nums) {
        return getMajority(nums, 0, nums.length - 1);
    }
}
```



---

**图解多数元素**到这就结束辣，大噶伙儿学废了嘛？

<div align=center>

![219b4197e057b1f9b856d2974abc4ea](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220122_214118966_0.jpg)

</div>