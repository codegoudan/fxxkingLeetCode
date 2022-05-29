大家好呀，我是帅蛋。

今天解决**左叶子之和**，一个理解对了左叶子概念就能理论 AC 的题，同样也可以反过来看，理解错了，WA 与你相随。

话不多说，我们搞起这道标号 404 ~

<div align=center>

![404-0](https://cdn.codegoudan.com/img/404-0.png)

</div>



# LeetCode 404：左叶子之和

题目链接：[左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)



## 题意

给定二叉树的根节点 root，返回所有左叶子之和。



## 示例

输入：root = [3,9,20,null,null,15,7]

输出：24

解释：在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24。

<div align=center>

![404-1](https://cdn.codegoudan.com/img/404-1.png)

</div>



## 提示

- 节点数在 [1,1000] 范围内
- -1000 <= Node.val <= 1000



# 题目解析

难度简单，只要明白了什么是左叶子节点，这道题就理论上解决了。

叶子节点是没有孩子节点的节点，那**左叶子节点就是“是左孩子且该左孩子没有孩子节点”**。

像下图，它的左叶子之和是 2。

<div align=center>

![404-2](https://cdn.codegoudan.com/img/404-2.png)

</div>



再比如下面这张图，它的左叶子之和呢？

<div align=center>

![404-3](https://cdn.codegoudan.com/img/404-3.png)

</div>

答案是 0，因为它没有左叶子节点。你肯定也答对了叭~

至于**具体的解法，其实就是遍历整棵树**：

- 如果遍历的当前节点的左孩子是一个叶子节点，则左孩子的值累加入结果。
- 如果遍历的当前节点的左孩子不是叶子节点，则继续遍历。



# 递归法

在既然要遍历二叉树，我们都知道二叉树有前中后序遍历及层次遍历，但是这里不推荐层次遍历。

因为篇幅原因，我这里以前序遍历为例，解决本题。不熟悉二叉树遍历（递归）的可以看下面这篇文章：



**[ACM 选手带你玩转二叉树前中后序遍历（递归版）](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)**



根据【[**递归算法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)】文章中讲的，实现递归，需要两步：

- 找出重复的子问题（递推公式）。
- 终止条件。

根据上面讲的实现递归的两步来实现：

**(1) 找出重复的子问题。**

这个很好找，前序遍历的顺序是：根、左子树、右子树。

对应到本题是：左叶子节点操作，递归左子树，递归右子树。

对于左子树或者右子树来说，也是同样的操作顺序。

所以这个重复的子问题就出来了，**先判断当前节点的左孩子是否为左叶子节点，再遍历左子树，最后遍历右子树**。

```Python
# 如果遍历到当前节点的左孩子为左叶节点，则将左孩子的值加入 res
if root.left != None and root.left.left == None and root.left.right == None:
    self.res += root.left.val
# 遍历左子树
self.sumOfLeftLeaves(root.left)
# 遍历右子树
self.sumOfLeftLeaves(root.right)
```

**(2) 确定终止条件。**

对于二叉树的遍历来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

```Python
# 递归中止条件
if root == None:
    return 0
```

这两点确定好了，代码也就出来了。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # 初始化 res
    def __init__(self):
        self.res = 0

    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        # 递归中止条件
        if root == None:
            return 0
        # 如果遍历到当前节点的左孩子为左叶节点，则将左孩子的值加入 res
        if root.left != None and root.left.left == None and root.left.right == None:
            self.res += root.left.val
        # 遍历左子树
        self.sumOfLeftLeaves(root.left)
        # 遍历右子树
        self.sumOfLeftLeaves(root.right)

        return self.res
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
    int res = 0;
    public int sumOfLeftLeaves(TreeNode root) {
        // 递归中止条件
        if(root == null){
            return 0;
        }
        // 如果遍历到当前节点的左孩子为左叶节点，则将左孩子的值加入 res
        if(root.left != null && root.left.left == null && root.left.right == null){
            res += root.left.val;
        }
        // 遍历左子树
        sumOfLeftLeaves(root.left);
        // 遍历右子树
        sumOfLeftLeaves(root.right);

        return res;
    }
}
```



本题解，每个节点都会被访问到，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

非递归的做法，又叫**迭代法**。**迭代法就是不断地将旧的变量值，递推计算新的变量值**。

**迭代法是用栈来做**，对二叉树前序遍历迭代法不太了解的蛋粉可以看下面这篇文章：



[**ACM 选手带你玩转二叉树前中后序遍历（非递归版）**](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



既然递归法用的是前序遍历，那迭代法我也用前序遍历来讲解。

根据栈“先入后出”的特点，结合前序遍历的顺序，迭代的过程其实很好理解：

**每次都是先将根节点放入栈，然后右子树，最后左子树。**

具体步骤如下所示：

- 初始化维护一个栈，将根节点入栈。

- 当栈不为空时，弹出栈顶元素 node：

- - 如果 node 的左孩子为叶子节点，该左孩子数值加入结果 res。
  - 如果 node 有右子树，则右子树入栈。
  - 如果 node 有左子树，则左子树入栈。

以下图为例：

<div align=center>

![404-4](https://cdn.codegoudan.com/img/404-4.png)

</div>

首先初始化栈 stack 和结果 res。

<div align=center>

![404-5](https://cdn.codegoudan.com/img/404-5.png)

</div>

```Python
# 初始化栈和结果
stack = [root]
res = 0
```

栈不为空，栈顶元素出栈 node = 3，它的左孩子为 9 且左孩子为叶子节点，将左孩子的值累加 res。

<div align=center>

![404-6](https://cdn.codegoudan.com/img/404-6.png)

</div>

```Python
# 弹出栈顶元素
node = stack.pop()
# 如果当前节点的左孩子为左叶节点
if node.left != None and node.left.left == None and node.left.right == None:
    res += node.left.val
```

接下来是 node 的右孩子入栈，左孩子入栈。

<div align=center>

![404-7](https://cdn.codegoudan.com/img/404-7.png)

</div>

```Python
# 右子树入栈
if node.right:
    stack.append(node.right)
# 左子树入栈
if node.left:
    stack.append(node.left)
```

当栈不为空时，接下来的操作都是在重复上述的内容。

<div align=center>

![404-8](https://cdn.codegoudan.com/img/404-8.png)

![404-9](https://cdn.codegoudan.com/img/404-9.png)

![404-10](https://cdn.codegoudan.com/img/404-10.png)

![404-11](https://cdn.codegoudan.com/img/404-11.png)

![404-12](https://cdn.codegoudan.com/img/404-12.png)

</div>

当栈为空时，结束，此时的 res = 24，即此二叉树的左叶子之和为 24。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        # 空树，直接返回 0
        if root == None:
            return 0
        # 初始化栈和结果
        stack = [root]
        res = 0
        # 当栈不为空
        while stack:
            # 弹出栈顶元素
            node = stack.pop()
            # 如果当前节点的左孩子为左叶节点
            if node.left != None and node.left.left == None and node.left.right == None:
                res += node.left.val
            # 右子树入栈
            if node.right:
                stack.append(node.right)
            # 左子树入栈
            if node.left:
                stack.append(node.left)

        return res
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
    public int sumOfLeftLeaves(TreeNode root) {
        // 空树，直接返回 0
        if(root == null){
            return 0;
        }
        // 初始化栈和结果
        Stack<TreeNode> stack = new Stack<> ();
        stack.add(root);
        int res = 0;
        // 当栈不为空
        while (!stack.isEmpty()) {
            // 弹出栈顶元素
            TreeNode node = stack.pop();
            // 如果当前节点的左孩子为左叶节点
            if (node.left != null && node.left.left == null && node.left.right == null) {
                res += node.left.val;
            }
            // 右子树入栈
            if (node.right != null){
                stack.add(node.right);
            }
            // 左子树入栈
            if (node.left != null){
                stack.add(node.left);
            }
        }
        return res;      
    }
}
```

同样迭代法，**时间复杂度为 O(n)，空间复杂度为 O(n)**。



---

**图解左叶子之和**到这就结束辣，没骗你们叭？左叶子节点懂了，一切就很顺畅了。

用二叉树的遍历去解题，是二叉树中很容易碰到的方法，大家还得多总结多思考。

今天就到这了，记得帮我**点赞 **呀，爱你们~

我是帅蛋，我们下次见！
