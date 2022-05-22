**滑动窗口最大值**这道题，**在 LeetCode 上有”滑动窗口“这四个字的基本上都是面试高频题**。

学过计算机网络的小婊贝估计都知道个滑动窗口协议，别慌，这道题没辣么难顶。

滑动窗口呢，一般就用在数组或者字符串上，我们先从字面上来认识一下滑动窗口：

- **滑动**：窗口可以按照一定的方向移动。
- **窗口**：窗口大小可以固定，也可以不固定，此时可以向外或者向内，扩容或者缩小窗口直至满足条件。

了解了这些，下面开始直接肝题。

<div align=center>

![239-0](https://cdn.codegoudan.com/img/239-0.png)

</div>



# LeetCode 239：滑动窗口最大值

题目链接：[滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)



## 题意

大小为 k 的滑动窗口从整数数组 nums 的最左侧移到最右侧，只能看到滑动窗口中的 k 个数字，窗口每次向右移动一位。

返回滑动窗口的最大值。



## 示例

<div align=center>

![239-1](https://cdn.codegoudan.com/img/239-1.png)

</div>



## 提示

- 1 <= nums.length <= 10^5

- -10^4 <= nums[i] <= 10^4

- 1 <= k <= nums.length



# 题目解析

滑动窗口最大值，是用队列解决的经典问题，难度困难。

## 单调队列

在开始之前我先介绍一个之前没有讲过的队列，叫**双端队列**。

普通队列是限制仅在队尾进行插入，在队头进行删除操作的线性表，队列的插入叫做入队列，队列的删除叫做出队列。

而**双端队列则是放开了这个限制，在队头和队尾两端都可以进行入队和出队操作的队列**。

<div align=center>

![239-2](https://cdn.codegoudan.com/img/239-2.png)

</div>

这么细看，其实对于队头或者队尾端，相当于是一个栈，后进的先出。

双端队列看上去这么的像栈和队列的结合体。

而有些时候，双端队列中还有受限的双端队列：一个是输出受限的双端队列，另一个是输入受限的双端队列。

**输出受限的双端队列**是：允许在一端进行入队和出队，但在另一端只允许入队的双端队列。

<div align=center>

![239-3](https://cdn.codegoudan.com/img/239-3.png)

</div>

**输入受限的双端队列**是：允许在一段进行入队和出队，但在另一端只允许出队的双端队列。

<div align=center>

![239-4](https://cdn.codegoudan.com/img/239-4.png)

</div>

输出受限的双端队列里，有一种情况，那就是队列里的各元素之间的关系具有单调性，这叫单调队列。

**单调队列，顾名思义，所有队列里的元素都是按递增（递减）的顺序队列，这个队列的头是最小（最大）的元素。**

哎呀妈呀，可算一步步的引出来了。

<div align=center>

![239-5](https://cdn.codegoudan.com/img/239-5.jpg)

</div>

这道题，就用队列中的双端队列中的单调队列来做，为什么能用这个呢？我们来顺一下做这道题的思路。



## 题解

首先窗口向右滑动的过程就是将窗口最左侧的元素删除，同时在窗口的最右侧添加一个新的元素，这就要用到双端队列，然后找双端队列中的最大元素。

那剩下就是如何找到滑动窗口中的最大值。

那我们就可以只在队列中保留可能成为窗口最大元素的元素，去掉不可能成为窗口中最大元素的元素。

想象一下，如果要进来的是个值大的元素，那一定会比之前早进去的值小的元素晚离开队列，而且值大的元素在，都没值小的元素啥事，所以值小的元素直接弹出队列即可。

这样队列里其实**维护的一个单调递减的单调队列**。



# 图解

注：**因为代码模拟方便，单调队列里存的是数组下标，在这为了演示的更加直观，图解中单调队列中用的是元素值**。

假设 nums = [1, 3, -1, -3, 5, 3, 6, 7]，k = 3。

首先初始化一个单调队列和结果数组。

<div align=center>

![239-6](https://cdn.codegoudan.com/img/239-6.png)

</div>

```Python
# 如果数组为空或 k = 0，直接返回空
if not nums or not k:
    return []
# 如果数组只有1个元素，直接返回该元素
if len(nums) == 1:
    return [nums[0]]

# 初始化队列和结果，队列存储数组的下标
queue = []
res = []
```

第 1 步，数值为 1，此时单调队列为空，直接入队列。

<div align=center>

![239-7](https://cdn.codegoudan.com/img/239-7.png)

</div>

第 2 步，数值为 3，queue 中有一个元素 1，3 > 1，所以按照我们之前说的，3 如果进入队列，那最大值只能会是 3，没有 1 的出头之日，所以 1 直接出队列即可。

<div align=center>

![239-8](https://cdn.codegoudan.com/img/239-8.png)

</div>

```Python
# 对于新进入的元素，如果队列前面的数比它小，那么前面的都出队列
while queue and nums[queue[-1]] < nums[i]:
    queue.pop()
# 新元素入队列
queue.append(i)
```

第 3 步，数值为 -1，-1 < 3，所以 -1 直接入队列。

此时走过了 k = 3 个元素，当前的最大值就是单调队列最左侧的值，也就是 3。

<div align=center>

![239-9](https://cdn.codegoudan.com/img/239-9.png)

</div>

```Python
# 当前的大值加入到结果数组中
if i >= k-1:
    res.append(nums[queue[0]])
```

第 4 步，窗口右移，此时对应下标数值为 -3，-3 < -1，所以 -3 直接入队列。

此时最大值依然还是 3。

<div align=center>

![239-10](https://cdn.codegoudan.com/img/239-10.png)

</div>

第 5 步，窗口右移，下标 1 对应的数值 3 不在窗口内，但是 3 在 queue 内，所以 3 出队列。

```Python
# 如果当前队列最左侧存储的下标等于 i-k 的值，代表目前队列已满。
# 但是新元素需要进来，所以列表最左侧的下标出队列
if queue and queue[0] == i - k:
    queue.pop(0)
```

此时下标对应的数值为 5，5 分别大于 -1 和 -3，所以 -1 和 -3 出队列，5 入队列，此时的最大值为 5。

<div align=center>

![239-11](https://cdn.codegoudan.com/img/239-11.png)

</div>

第 6 步，窗口右移，当前下标对应的数值为 3，3 < 5，所以 3 直接入队列，此时的最大值是 5。

<div align=center>

![239-12](https://cdn.codegoudan.com/img/239-12.png)

</div>

第 7 步，窗口右移，当前下标对应的数值为 6，6 大于 queue 中的 5 和 3，所以 queue 中的 5 和 3 出队列，然后 6 入队列，此时最大的值为 6。

<div align=center>

![239-13](https://cdn.codegoudan.com/img/239-13.png)

</div>

第 8 步，窗口右移，当前下标对应的数值为 7，7 > 6，所以 queue 中 6 出队列，7 入队列，当前最大值为 7。

<div align=center>

![239-14](https://cdn.codegoudan.com/img/239-14.png)

</div>

此时数组扫描完，直接输出 res 的结果即可。

本题的解法，数组被从头到尾扫描一遍，每个元素的下标最多进出队列一次，所以**时间复杂度为 O(n)**。

因为创建了一个额外的大小为 k 的队列存储，所以本题**空间复杂度为 O(k)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

        # 如果数组为空或 k = 0，直接返回空
        if not nums or not k:
            return []
        # 如果数组只有1个元素，直接返回该元素
        if len(nums) == 1:
            return [nums[0]]

        # 初始化队列和结果，队列存储数组的下标
        queue = []
        res = []

        for i in range(len(nums)):
            # 如果当前队列最左侧存储的下标等于 i-k 的值，代表目前队列已满。
            # 但是新元素需要进来，所以列表最左侧的下标出队列
            if queue and queue[0] == i - k:
                queue.pop(0)

            # 对于新进入的元素，如果队列前面的数比它小，那么前面的都出队列
            while queue and nums[queue[-1]] < nums[i]:
                queue.pop()
            # 新元素入队列
            queue.append(i)

            # 当前的大值加入到结果数组中
            if i >= k-1:
                res.append(nums[queue[0]])

        return res
```



## Java 代码实现

```Java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        if(nums == null || nums.length < 2) return nums;
        // 双端队列 保存当前窗口最大值的数组位置 保证队列中数组位置的数值按从大到小排序
        LinkedList<Integer> queue = new LinkedList();
        // 结果数组
        int[] result = new int[nums.length-k+1];
        for(int i = 0;i < nums.length;i++){
            // 保证从大到小 如果前面数小则需要依次弹出，直至满足要求
            while(!queue.isEmpty() && nums[queue.peekLast()] <= nums[i]){
                queue.pollLast();
            }
            // 添加当前值对应的数组下标
            queue.addLast(i);
            // 判断当前队列中队首的值是否有效
            if(queue.peek() <= i-k){
                queue.poll();   
            } 
            // 当窗口长度为k时 保存当前窗口中最大值
            if(i+1 >= k){
                result[i+1-k] = nums[queue.peek()];
            }
        }
        return result;
    }
}
```



---

**图解滑动窗口最大值**就到这了，

哎呀妈呀，可写完了，还画了好多丑丑的图，差点累死。

你看虽然这是一道难度困难的题，但是思路搞明白了，一切都顺理成章。

**AC 不是目的，目的是好好去琢磨琢磨这种解题方法，然后能运用到之后的题目中。**

