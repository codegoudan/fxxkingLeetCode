大家好，我是帅蛋。

今天解决**最大二叉树**，这是一道构造二叉树的题，只不过是按照题目的规则来构造。

我们之前做过【[**前 + 中 -> 二叉树**](https://mp.weixin.qq.com/s/lM4_ntDfpoQRhbeXpCZaKw)】、【[**中 +后 -> 二叉树**](https://mp.weixin.qq.com/s/5kmedNUMkKSsqZiC1OtYdA)】，最大二叉树也大抵如此，这种问题本身还是考察对**二叉树遍历**的掌握程度。

<div align=center>

![654-0](https://cdn.codegoudan.com/img/654-0.png)

</div>



#  **LeetCode 654：最大二叉树**



## 题意

不重复的整数数组 nums。最大二叉树可以用下面的算法从 nums 递归地构建：

创建一个根节点，其值为 nums 中地最大值。

递归地在最大值左边地子数组前缀上构建左子树。

递归地在最大值右边地子数组后缀上构建右子树。

返回 nums 构建地最大二叉树。



## 示例

输入：nums = [3,2,1,6,0,5]
输出：[6,3,5,null,2,0,null,null,1]

<div align=center>

![654-1](https://cdn.codegoudan.com/img/654-1.png)

</div>



## 提示

- 1 <= nums.length <= 1000
- 0 <= nums[i] <= 1000
- nums 中的所有整数互不相同



# 题目解析

最大二叉树这道题 LeetCode 定义难度为中等。呃，说实话，我没搞明白难度在哪，我感觉就是道简单题。

至于解法我就不卖关子了，这是一道极其典型的使用**分治算法**求解的题目。

如果还不了解分治算法，可以先看下面这篇文章：



**[ACM 选手带你玩转分治算法！](https://mp.weixin.qq.com/s/_9o3EShFRTa6odf5qEnCtw)**



**分治算法由“分”和“治”两部分组成**，但是它主要包括 **3 个过程**：

- 划分（Divide）
- 求解（Conquer）
- 合并（Combine）

其中：

**划分（Divide）**：将原问题划分为规模较小的子问题，子问题相互独立，与原问题形式相同。

**求解（Conquer）**：递归的求解划分之后的子问题。

**合并（Combine）**：这一步非必须。有些问题涉及合并子问题的解，将子问题的解合并成原问题的解。有的问题则不需要，只是求出子问题的解即可。

<div align=center>

![654-7](https://cdn.codegoudan.com/img/654-7.png)

</div>

说白了就是**先找找拆分到最小规模问题时怎么解，然后再瞅瞅随着问题规模增大点问题怎么解，最后就是找到递归函数，码出递归代码即可**。

在本题中，其实只需要前两步：划分和求解。



**(1) 划分**

划分就是拆解到问题的最小规模。

在本问题中拆解是个很简单的事，每次找到区间里的最大值，最大值左边为左子树，右边为右子树（其实就是拆成了两个区间）。

然后左子树和右子树还是按照同样的方式拆解，直至拆成最小规模的问题，即区间内没有数为止。

但是你仔细品，上面找最大值构造根节点，然后再左子树，最后右子树的形式你有没有觉得很熟？

这不就是**前序遍历**的方式嘛，又破案了...



**(2) 求解**

递归的求解划分之后的子问题。

最小的情况就是，当右边界 > 左边界，即区间内没有数了，也就结束了。



# 图解

以 nums = [3,2,1,6,0,5] 为例。

首先初始化当前 nums 最大值所在的下标 maxIndex = 0，start = 0，end = len(nums) = 6。

<div align=center>

![654-2](https://cdn.codegoudan.com/img/654-2.png)

</div>

```Python
# 初始化最大值下标
maxIndex = start
```



第 1 步，找到当前 nums 的最大值的下标 maxIndex = 3。

<div align=center>

![654-3](https://cdn.codegoudan.com/img/654-3.png)

</div>

```Python
# 找到最大值的下标
for i in range(start + 1, end):
    if nums[i] > nums[maxIndex]:
        maxIndex = i
```



当前最大值下标对应的就是最大值，即最大二叉树的根节点的值，此时创建根节点。

<div align=center>

![654-4](https://cdn.codegoudan.com/img/654-4.png)

</div>

```Python
# 构建根节点
root = TreeNode(nums[maxIndex])
```



此时就区分出了左子树（即左区间）和右子树（即右区间），左区间的范围为[0,3)，右区间的范围为[4, 6)。

<div align=center>

![654-5](https://cdn.codegoudan.com/img/654-5.png)

</div>

接下来就还是按照上面的方式，对左子树和右子树进行操作。

```Python
# 递归左子树
root.left = self.maxBinaryTree(nums, start, maxIndex)
# 递归右子树
root.right = self.maxBinaryTree(nums, maxIndex + 1, end)
```



直至分无可分。

```Python
# 区间内没有数字，返回 None
if start == end:
    return None
```



最终最大二叉树就成了下图的样子。

<div align=center>

![654-6](https://cdn.codegoudan.com/img/654-6.png)

</div>

对于本题解，最坏情况下数组为单调数组，此时递归 n 次，每次递归查找最大值的时间复杂度为 O(n)，即**总的时间复杂度为 O(n^2)**。

此外递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 代码实现



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxBinaryTree(self, nums:List[int], start, end):
        # 区间内没有数字，返回 None
        if start == end:
            return None
        # 初始化最大值下标
        maxIndex = start
        # 找到最大值的下标
        for i in range(start + 1, end):
            if nums[i] > nums[maxIndex]:
                maxIndex = i

        # 构建根节点
        root = TreeNode(nums[maxIndex])

        # 递归左子树
        root.left = self.maxBinaryTree(nums, start, maxIndex)
        # 递归右子树
        root.right = self.maxBinaryTree(nums, maxIndex + 1, end)

        return root

    def constructMaximumBinaryTree(self, nums: List[int]) -> TreeNode:
        return self.maxBinaryTree(nums, 0, len(nums))
```



## Java 代码实现

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public TreeNode maxBinaryTree(int[] nums, int start, int end){
        // 区间内没有数字，返回 None
        if(start == end){
            return null;
        }
        // 初始化最大值下标
        int maxIndex = start;
        // 找到最大值的下标
        for(int i = start + 1; i < end; i ++){
            if(nums[i] > nums[maxIndex]){
                maxIndex = i;
            }
        }
        // 构建根节点
        TreeNode root = new TreeNode(nums[maxIndex]);
        // 递归左子树
        root.left = maxBinaryTree(nums, start, maxIndex);
        // 递归右子树
        root.right = maxBinaryTree(nums, maxIndex + 1, end);

        return root;
        
    }

    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return  maxBinaryTree(nums, 0, nums.length);
    }
}
```



---

**图解最大二叉树**到这就结束辣，是不是就是道简单题，没骗你们叭？

但是你没觉得很有意思么？我们用的分治算法，用着用着，这不又是个前序遍历的方式嘛！

真是有意思的二叉树！

希望大家能多多思考，做题的时候不要做完了就算了。

我是帅蛋，我们下次见咯~
