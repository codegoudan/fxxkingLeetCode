大家好呀，我是帅蛋。

今天解决**路径总和**，如果你看过我【[**二叉树的所有路径**](https://mp.weixin.qq.com/s/NdZFFlGrOA7bcUgK6I1nfw)】这篇文章，你会发现这其实是在它解法的基础上做了一些改动。

咱们话不多说，直接开整。

<div align=center>

![112-0](https://cdn.codegoudan.com/img/112-0.png)

</div>



# **LeetCode 112：路径总和**

题目链接：[ 路径总和](https://leetcode.cn/problems/path-sum/)



## 题意

给定二叉树的根节点 root 和一个表示目标的整数 targetSum。

判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和 targetSum。存在返回 true，否则返回 false。



## 示例

输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22

输出：true

解释：等于目标和的根节点到叶节点路径如下图所示。

<div align=center>

![112-1](https://cdn.codegoudan.com/img/112-1.png)

</div>



## 提示

- 树中节点的数目在范围 [0， 5000] 内
- -1000 <= Node.val <= 1000
- -1000 <= targetSum <= 1000



# 题目解析

难度简单，如果你看过我【[**二叉树的所有路径**](https://mp.weixin.qq.com/s/dszDxqVq1DdUEK_45Ugk0Q)】这篇文章，思路应该从你脑阔里立马跳出来。

如果你看过我足够多二叉树的文章，那你应该知道这个思路叫：自顶向下。

我在这里自顶向下用的其实就是前序遍历的方式，每次先判断当前的节点，再递归左子树，最后是右子树。

在本题路径总和中，自顶向下的解法其实就是：

- 判断当前节点是否为叶子节点，如果是叶子节点，判断当前叶子节点的值是否 = targetSum 减去之前路径上节点值。
- 递归左子树。
- 递归右子树。

这种每次先根节点，再左子树，最后右子树，典型的前序遍历的方式。

<div align=center>

![112-2](https://cdn.codegoudan.com/img/112-2.png)

</div>



# 递归法

用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是按照往常，祭出递归二步曲：



**(1) 找出重复的子问题。**

前序遍历的顺序是：根节点、左子树、右子树。

在本题同样也是这个顺序：**判断节点，递归左子树，递归右子树**。

对于左子树和右子树来说，也都是同样的操作。

```Python
# 递归左子树
leftPath = self.hasPathSum(root.left, targetSum - root.val)
# 递归右子树
rightPath = self.hasPathSum(root.right, targetSum - root.val)
```



**(2) 确定终止条件。**

对于路径来说，遍历到叶子节点且叶子节点的值等于该路径之前节点的值，证明已找到，返回 True。

```Python
# 如果当前节点为叶子节点，且叶子节点的值等于减去该路径之前节点的值，返回 True
if root.left == None and root.right == None and root.val == targetSum:
    return True
```



最重要的两点确定完了，那总的代码也就出来了。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:

    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        # 如果树为空，返回 False
        if root == None:
            return False
        # 如果当前节点为叶子节点，且叶子节点的值等于减去该路径之前节点的值，返回 True
        if root.left == None and root.right == None and root.val == targetSum:
            return True
        # 递归左子树
        leftPath = self.hasPathSum(root.left, targetSum - root.val)
        # 递归右子树
        rightPath = self.hasPathSum(root.right, targetSum - root.val)
        
        return leftPath or rightPath
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
    public boolean hasPathSum(TreeNode root, int targetSum) {
        // 如果树为空，返回 False
        if(root == null){
            return false;
        }
        // 如果当前节点为叶子节点，且叶子节点的值等于减去该路径之前节点的值，返回 True
        if(root.left == null && root.right == null && root.val == targetSum){
            return true;
        }
        // 递归左子树
        boolean leftPath = hasPathSum(root.left, targetSum - root.val);
        // 递归右子树
        boolean rightPath = hasPathSum(root.right, targetSum - root.val);

        return leftPath || rightPath;
    }
}
```



本题解，最坏情况下，每个节点都会被访问到，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

非递归的做法，很多人又叫**迭代法**。**迭代法就是不断地将旧的变量值，递推计算新的变量值**。

**迭代法是用栈来做**，对二叉树前序遍历迭代法不太了解的蛋粉可以看下面这篇文章：



[**ACM 选手带你玩转二叉树前中后序遍历（非递归版）**](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



根据栈“先入后出”的特点，结合前序遍历的顺序，迭代的过程也就出来了：

**每次都是先将节点放入栈，然后右子树，最后左子树。**

具体步骤如下所示：

- 初始化维护栈，将根节点及根节点的值入栈。

- 当栈不为空时，弹出栈顶的节点及节点的值：

- - 若当前节点为叶子节点且累积值 val 为 targetSum，返回 true。
  - 若当前节点的右子树不为空，将右孩子及 “val + 右孩子的值”入栈。
  - 若当前节点的左子树不为空，将左孩子及”val + 左孩子的值“入栈。

以下图为例：

<div align=center>

![112-3](https://cdn.codegoudan.com/img/112-3.png)

</div>

首先初始化栈 stack。

<div align=center>

![112-4](https://cdn.codegoudan.com/img/112-4.png)

</div>

```Python
# 初始化栈
stack = [(root, root.val)]
```



当栈不为空，栈顶元素 node 和栈顶值 val 出栈。

<div align=center>

![112-5](https://cdn.codegoudan.com/img/112-5.png)

</div>

```Python
node, val = stack.pop()
```



此时 node = 5，val = 5，此时 val 不等于 targetSum，此时 node 有左右子树，此时”右子树及路径值“先入栈，再”左子树及路径值“入栈。

<div align=center>

![112-6](https://cdn.codegoudan.com/img/112-6.png)

</div>

```Python
# 右子树入栈
if node.right:
    stack.append((node.right, val + node.right.val))
# 左子树入栈
if node.left:
    stack.append((node.left, val + node.left.val))
```



接下来其实就是在重复上面的操作。

弹出栈顶元素 node = 4， val = 9，此时 val 不等于 targetSum，此时 node 有左子树，此时”左子树及路径值“入栈。

<div align=center>

![112-7](https://cdn.codegoudan.com/img/112-7.png)

</div>

弹出栈顶元素 node =11， val = 20，此时 val 不等于 targetSum，此时 node 有左右子树，此时”右子树及路径值“先入栈，再”左子树及路径值“入栈。

<div align=center>

![112-8](https://cdn.codegoudan.com/img/112-8.png)

</div>

弹出栈顶元素 node =7， val = 27，此时 node = 7 为叶子节点，但 val 不等于 targetSum。

<div align=center>

![112-9](https://cdn.codegoudan.com/img/112-9.png)

</div>

继续弹出栈顶元素 node = 2，val = 22，此时 node 为叶子节点，且 val 等于 targetSum，证明该树中存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和 targetSum。

<div align=center>

![112-10](https://cdn.codegoudan.com/img/112-10.png)

</div>

```Python
# 如果当前节点为叶子节点且路径总和 = targetSum
if node.left == None and node.right == None and val == targetSum:
    return True
```



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        # 如果树为空，返回 False
        if root == None:
            return False
        # 初始化栈
        stack = [(root, root.val)]

        while stack:
            node, val = stack.pop()
            # 如果当前节点为叶子节点且路径总和 = targetSum
            if node.left == None and node.right == None and val == targetSum:
                return True
            # 右子树入栈
            if node.right:
                stack.append((node.right, val + node.right.val))
            # 左子树入栈
            if node.left:
                stack.append((node.left, val + node.left.val))
            
        return False
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
    public boolean hasPathSum(TreeNode root, int targetSum) {
        // 如果树为空，返回 False
        if(root == null){
            return false;
        }
        // 初始化栈
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> stackVal = new Stack<>();
        stack.push(root);
        stackVal.push(root.val);

        while(!stack.isEmpty()){
            int n = stack.size();
            for(int i = 0; i < n; i++){
                TreeNode node = stack.pop();
                int val = stackVal.pop();
                // 如果当前节点为叶子节点且路径总和 = targetSum
                if(node.left == null && node.right == null && val == targetSum){
                    return true;
                }
                // 右子树入栈
                if(node.right != null){
                    stack.push(node.right);
                    stackVal.push(val + node.right.val);
                }
                // 左子树入栈
                if(node.left != null){
                    stack.push(node.left);
                    stackVal.push(val + node.left.val);
                }
            }
        }
        return false;
    }
}
```



同样，本题解**时间复杂度为 O(n)**，**空间复杂度为 O(n)**。



---

**图解路经总和**到这就结束辣，你学废了嘛？

你看吧，其实数据结构与算法题啊，刷多了你就会发现似曾相识，切起题来就会一顺百顺。

这个道理我都交给你了，这不得马上安排个**点赞**嘛。

哈哈哈哈，我是帅蛋，我们下次见！
