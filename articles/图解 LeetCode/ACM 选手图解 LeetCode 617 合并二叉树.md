大家好呀，我是帅蛋。

今天解决**合并二叉树**，将两棵二叉树根据给出的规则合并成一棵二叉树。

咱们话不多说，直接开整。

<div align=center>

![617-0](https://cdn.codegoudan.com/img/617-0.png)

</div>



# **LeetCode 617：合并二叉树**

题目链接：[合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)



## 题意

给定两棵二叉树 root1 和 root2，合并成一棵新的二叉树，合并规则为：

如果两个节点重叠，将两个节点的值相加作为合并后节点的新值，否则，不为 null 的节点将直接作为新二叉树的节点。



## 示例

输入：root1 = [1,3,2,5]，root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]

<div align=center>

![617-1](https://cdn.codegoudan.com/img/617-1.png)

</div>



## 提示

合并过程必须从两个树的根节点开始。

- 两棵树中的节点数目在范围 [0,2000] 内
- -10^4 <= Node.val <= 10^4



# 题目解析

难度简单，二叉树遍历解法经典题目。

合并二叉树这道题，具体做法其实都已经在描述里了，其实就是**分两种情况讨论**：

- 如果两棵树对应位置上都有节点，则新节点的值为两个节点的值相加。
- 如果两棵树对应位置上只有一个节点有值，则新节点的值就为该节点的值。

其实就是遍历两棵二叉树，在这个过程中遵守上面的两种情况即可。

我用颜色稍微标记一下，可能看的更清楚一些：

<div align=center>

![617-2](https://cdn.codegoudan.com/img/617-2.png)

</div>



# 递归法

递归法就是老套路，根据【[**递归算法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)】文章中讲的，实现递归，需要两步：

- 找出重复的子问题（递推公式）。
- 终止条件。



**(1) 找出重复的子问题。**

这个在上面看图的时候其实已经找出来了。

对于每一层来说，就是重新创建一个节点 root 存储 root1.val + root2.val。

之后 root 的左子树就是合并 root1 左子树和 root2 左子树之后的左子树，root 的右子树就是合并 root1 右子树和 root2 右子树之后的右子树。

```Python
# 如果都存在节点，创建一个新的节点存储合并后的值
root = TreeNode(root1.val + root2.val)
# 递归合并左子树
root.left = self.mergeTrees(root1.left, root2.left)
# 递归合并右子树
root.right = self.mergeTrees(root1.right, root2.right)
```



你看这种先根节点再左子树最后右子树的方式是不是很眼熟？这就是我们学的**前序遍历**。

当然像中序遍历或者后序遍历的方式都是可以的，我在这就不多赘述，大家可以自己写着试试。



**(2) 确定终止条件。**

对于中止条件的话，在本题中就是：

对于遍历的两棵二叉树 root1 和 root2，如果 root1 的节点为空，那合并之后就应该是 root2，同样如果 root2 的节点为空，那合并之后就应该是 root1。

```Python
// 如果 root1 为空，则合并之后节点为 root2
if(root1 == null){
    return root2;
}
// 如果 root2 为空，则合并之后节点为 root1
if(root2 == null){
    return root1;
}
```



递归二步曲确定完了，这道题的代码其实也就做出来了。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:
        # 如果 root1 为空，则合并之后节点为 root2
        if root1 == None:
            return root2
        # 如果 root2 为空，则合并之后节点为 root1
        if root2 == None:
            return root1
        # 如果都存在节点，创建一个新的节点存储合并后的值
        root = TreeNode(root1.val + root2.val)
        # 递归合并左子树
        root.left = self.mergeTrees(root1.left, root2.left)
        # 递归合并右子树
        root.right = self.mergeTrees(root1.right, root2.right)

        return root
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
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        // 如果 root1 为空，则合并之后节点为 root2
        if(root1 == null){
            return root2;
        }
        // 如果 root2 为空，则合并之后节点为 root1
        if(root2 == null){
            return root1;
        }
        // 如果都存在节点，创建一个新的节点存储合并后的值
        TreeNode root = new TreeNode(root1.val + root2.val);
        // 递归合并左子树
        root.left = mergeTrees(root1.left, root2.left);
        // 递归合并右子树
        root.right = mergeTrees(root1.right, root2.right);

        return root;
    }
}
```



假设 root1 的节点数为 n，root2 的节点数为 m，对于两棵二叉树而言，只有对应的节点都不为空时才会进行合并操作，所以合并的时候一定是遍历完了节点数相对较少的二叉树，所以**总的时间复杂度为****O(min(n,m))**。

 此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树 root1 最坏情况下的高度为 n，二叉树 root2 最坏情况下的高度为 m，所以**空间复杂度为 O(min(n,m))**。



# 非递归法（迭代）

在递归法中，我们在找重复子问题的时候，对于每一层来说，都是看 root1 的左子树和 root2 的左子树，以及 root1 的右子树和 root2 的右子树。

这种一层层的看下去有点类似**层次遍历**。

我在【[**层次遍历**](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)】文章中说过，**非递归版的层次遍历用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

在本题中，具体的做法就是维护两个队列，队列 queueMerge 存储合并后的树节点，queue 则是 root1 和 root2 的节点，具体做法我们来图解。

比如对于下图，左边是 root1，右边是 root2：

<div align=center>

![617-3](https://cdn.codegoudan.com/img/617-3.png)

</div>

首先我们先创建一个新的根节点 root，其值为 root1.val + root2.val，同时初始化两个队列，将 root 入 queueMerge 队列，将 root1 和 root2 入 queue 队列。

<div align=center>

![617-4](https://cdn.codegoudan.com/img/617-4.png)

</div>

```Python
# 如果都存在节点，创建一个新的节点存储合并后的值
root = TreeNode(root1.val + root2.val)
# 初始化队列
queueMerge = [root]
queue = [root1, root2]
```



此时队列不为空，将 queueMerge 队首元素出队列，此时 node = 3，同时将 queue 两个队首元素出队列，此时 node1 = 1，node2 = 2。

对于 node1 和 node2，都存在左孩子，根据题目的描述，node 的左孩子为 node1 左孩子的值 + node2 左孩子的值。

同时将 node 的左孩子入 queueMerge 队列，将 node1 的左孩子和 node2 的左孩子入 queue 队列。

<div align=center>

![617-5](https://cdn.codegoudan.com/img/617-5.png)

</div>

```Python
# 从队列中取出当前节点
node = queueMerge.pop(0)
node1 = queue.pop(0)
node2 = queue.pop(0)
if node1.left or node2.left:
    # 若两棵树的左孩子都存在
    if node1.left and node2.left:
        leftMerge = TreeNode(node1.left.val + node2.left.val)
        node.left = leftMerge
        queueMerge.append(leftMerge)
        queue.append(node1.left)
        queue.append(node2.left)
```



同样，对于 node1 和 node2 来说，都存在右孩子， 根据题目的描述，node 的右孩子为 node1 右孩子的值 + node2 右孩子的值。

同时将 node 的右孩子入 queueMerge 队列，将 node1 的右孩子和 node2 的右孩子入 queue 队列。

<div align=center>

![617-6](https://cdn.codegoudan.com/img/617-6.png)

</div>

```Python
if node1.right or node2.right:
    # 若两棵树的右孩子都在
    if node1.right and node2.right:
        rightMerge = TreeNode(node1.right.val + node2.right.val)
        node.right = rightMerge
        queueMerge.append(rightMerge)
        queue.append(node1.right)
        queue.append(node2.right)
```



队列不为空，继续出队列，root = 4，root1 = 3， root2 = 1。

此时 root1 有左孩子，root2 没有左孩子，则 root 的左孩子直接为 root1 的左孩子。

<div align=center>

![617-7](https://cdn.codegoudan.com/img/617-7.png)

</div>

```Python
elif node1.left:
    node.left = node1.left
```



同时此时 root1 没有右孩子，root2 有右孩子，则 root 的右孩子直接为 root2 的右孩子。

<div align=center>

![617-8](https://cdn.codegoudan.com/img/617-8.png)

</div>

```Python
elif node2.right:
    node.right = node2.right
```



接下来就是重复上面的操作，直至队列为空，返回最后的结果。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:
        # 如果 root1 为空，则合并之后节点为 root2
        if root1 == None:
            return root2
        # 如果 root2 为空，则合并之后节点为 root1
        if root2 == None:
            return root1
        # 如果都存在节点，创建一个新的节点存储合并后的值
        root = TreeNode(root1.val + root2.val)
        # 初始化队列
        queueMerge = [root]
        queue = [root1, root2]
        
        while queue:
            # 从队列中取出当前节点
            node = queueMerge.pop(0)
            node1 = queue.pop(0)
            node2 = queue.pop(0)
            if node1.left or node2.left:
                # 若两棵树的左孩子都存在
                if node1.left and node2.left:
                    leftMerge = TreeNode(node1.left.val + node2.left.val)
                    node.left = leftMerge
                    queueMerge.append(leftMerge)
                    queue.append(node1.left)
                    queue.append(node2.left)
                # 若只有一棵树存在左孩子，直接赋值
                elif node1.left:
                    node.left = node1.left
                elif node2.left:
                    node.left = node2.left
            if node1.right or node2.right:
                # 若两棵树的右孩子都在
                if node1.right and node2.right:
                    rightMerge = TreeNode(node1.right.val + node2.right.val)
                    node.right = rightMerge
                    queueMerge.append(rightMerge)
                    queue.append(node1.right)
                    queue.append(node2.right)
                # 若只有一棵树存在右孩子，直接赋值
                elif node1.right:
                    node.right = node1.right
                elif node2.right:
                    node.right = node2.right
        return root
```



## Java 代码实现

```java
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
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        // 如果 root1 为空，则合并之后节点为 root2
        if(root1 == null){
            return root2;
        }
        // 如果 root2 为空，则合并之后节点为 root1
        if(root2 == null){
            return root1;
        }
        // 如果都存在节点，创建一个新的节点存储合并后的值
        TreeNode root = new TreeNode(root1.val + root2.val);
        // 初始化队列
        Queue<TreeNode> queueMerge = new LinkedList<TreeNode>();
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queueMerge.offer(root);
        queue.offer(root1);
        queue.offer(root2);

        while(!queue.isEmpty()){
            // 从队列中取出当前节点
            TreeNode node = queueMerge.poll();
            TreeNode node1 = queue.poll(), node2 = queue.poll();
            if(node1.left != null || node2.left != null){
                // 若两棵树的左孩子都存在
                if(node1.left != null && node2.left != null){
                    TreeNode leftMerge = new TreeNode(node1.left.val + node2.left.val);
                    node.left = leftMerge;
                    queueMerge.offer(leftMerge);
                    queue.offer(node1.left);
                    queue.offer(node2.left);
                }
                // 若只有一棵树存在左孩子，直接赋值
                else if(node1.left != null){
                    node.left = node1.left;
                }
                else if(node2.left != null){
                    node.left = node2.left;
                }
            }
            //  若两棵树的右孩子都在
            if(node1.right != null || node2.right != null){
                if(node1.right != null && node2.right != null){
                    TreeNode rightMerge = new TreeNode(node1.right.val + node2.right.val);
                    node.right = rightMerge;
                    queueMerge.offer(rightMerge);
                    queue.offer(node1.right);
                    queue.offer(node2.right);
                }
                // 若只有一棵树存在右孩子，直接赋值
                else if(node1.right != null){
                    node.right = node1.right;
                }
                else if(node2.right != null){
                    node.right = node2.right;
                }
            }
        }
        return root;
    }
}
```



同样，对于非递归法，它**总的时间复杂度为O(min(n,m))，空间复杂度为 O(min(n,m))**。



---

**图解合并二叉树**到这就结束辣，大家学会了嘛？是不是大喊一声，二叉树遍历，YYDS！

这道题我在写的时候就觉得贼神奇，递归法几行代码，非递归法五六十行，累了累了。

今天就先到这，记得做好总结，还有...本蛋的点赞~

我是帅蛋，我们下次见！
