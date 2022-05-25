大家好呀，我是帅蛋。

今天解决**二叉树的最近公共祖先**，很多同学在看到这个题目的时候，可能第一反应就是“卧槽？这个我能会？”。

还没开始就先把自己劝退了。

<div align=center>

![236-4](https://cdn.codegoudan.com/img/236-4.jpg)

</div>

但是现在二叉树讲到现在，已经做了这么多题，你只要是好好跟下来的，其实只要不是特别难的题，仔细分析分析就发现解法都似曾相识。

话不多说，让我们赶紧开始~

<div align=center>

![236-0](https://cdn.codegoudan.com/img/236-0.png)

</div>



# LeetCode 236：二叉树的最近公共祖先



## 题意

给定一个二叉树，找到该树中两个指定节点的最近公共祖先。

最近公共祖先：对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的最先且 x 的深度尽可能的大（一个节点也可以是它自己的祖先）。



## 示例

输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4

输出：5

解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。

<div align=center>

![236-1](https://cdn.codegoudan.com/img/236-1.png)

</div>

## 提示

- 树中节点数目在范围 [2, 10^5] 内。
- -10^9 <= Node.val <= 10^9
- 所有的 Node.val 互不相同。
- p ！= q
- p 和 q 均存在于给定的二叉树中。



# 题目解析

难度中等，超级经典的题目！我想先从概念入手给大家做个剖析。

首先我们来看什么是祖先。

**祖先其实就是从根节点到当前节点所经过的所有节点**，以下图为例：

<div align=center>

![236-2](https://cdn.codegoudan.com/img/236-2.png)

</div>

根据概念，对于节点 7 的祖先其实是 3、5、2、7。

然后我们再来看最近公共祖先的概念，我感觉官方在问题描述中对于这个概念介绍的有点难懂...

**最近公共祖先**，我感觉你们就记住，**对于节点 p 和 q 来说，如果 node 为其最近公共祖先，那么 node 的左孩子和右孩子一定不是 p 和 q 的公共祖先**。

比如对于上图， p = 7、q = 0 来说，3 就是 p 和 q 的最近公共祖先（3 的左孩子 5 和右孩子 1 都不是 p 和 q 的公共祖先）。

<div align=center>

![236-3](https://cdn.codegoudan.com/img/236-3.png)

</div>

根据这个我们可以得出，如果节点 node 为 p 和 q 的最近公共祖先，那么会有 **3** 种情况：

- p 和 q 分别在节点 node 的左右子树中。
- node 即为节点 p，q 在节点 p 的左子树或右子树中。
- node 即为节点 q，p 在节点 q 的左子树或者右子树中。

这样我们可以使用**递归法**解决。

既然是用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是按照往常，祭出递归二步曲：



**(1) 找出重复的子问题。**

这里的重复子问题就很简单，就是递归左子树，递归右子树，然后处理逻辑。

```Python
# 递归左右子树，保存递归结果
left = self.lowestCommonAncestor(root.left, p, q)
right = self.lowestCommonAncestor(root.right, p, q)
```



你看这种**先左子树，再右子树，最后处理节点的顺序，典型的后序遍历**。



**(2) 确定终止条件。**

对于本题来说，终止条件还是有点多的。

我们在递归 left 和 right 之后：

如果 left 和 right 都非空，那证明 p 和 q 一边一个，那么最近公共祖先就是 root，直接返回 root。

```python
# 如果 left 和 right 都非空，那证明 p 和 q 一边一个，那么最近公共祖先就是 root
if left and right:
      return root
```



如果 right 为空，left 不为空，那只需要看 left。

```Python
# 如果 right 为空，只需要看 left
if left and right == None:
      return left
```



如果 left 为空，right 不为空，那只需要看 right。

```Python
# 如果 left 为空，只需要看 right 
if left == None and right:
      return right
```



如果都为空，那就直接返回空。

```Python
# 如果都为空，返回空
if left == None and right == None:
    return None
```



这两点确定好了，最后的代码也就出来了。



# 代码实现



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 当前节点为空，直接返回空
        if root == None:
            return None
        # 如果 root 等于 p 或者 q，那最近公共祖先一定是 p 或者 q
        if root == p or root == q:
            return root
        # 递归左右子树，保存递归结果
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        # 如果 left 和 right 都非空，那证明 p 和 q 一边一个，那么最近公共祖先就是 root
        if left and right:
            return root
        # 如果 right 为空，只需要看 left
        if left and right == None:
            return left
        # 如果 left 为空，只需要看 right 
        if left == None and right:
            return right
        # 如果都为空，返回空
        if left == None and right == None:
            return None
```



## Java 代码实现

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 当前节点为空，直接返回空
        if(root == null){
            return null;
        }
        // 如果 root 等于 p 或者 q，那最近公共祖先一定是 p 或者 q
        if(root == p || root == q){
            return root;
        }
        // 递归左右子树，保存递归结果
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        // 如果 left 和 right 都非空，那证明 p 和 q 一边一个，那么最近公共祖先就是 root
        if(left != null && right != null){
            return root;
        }
        // 如果 right 为空，只需要看 left
        else if(left != null && right == null){
            return left;
        }
        // 如果 left 为空，只需要看 right
        else if(left == null && right != null){
            return right;
        }
        // 如果都为空，返回空
        else{
            return null;
        }
    }
}
```



本题解，在递归过程中最坏情况下每个节点都被遍历到，**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



---

**图解二叉树的最近公共祖先**到这就结束辣，你看我们只需要从概念入手，一点点的剖析，解法就出来了。

还是老生常谈的那句话，**题目的解决都是从我们过去学过的知识中寻找办法**。

今天就到这了，记得帮我**点赞**呀，爱你们~

我是帅蛋，我们下次见！
