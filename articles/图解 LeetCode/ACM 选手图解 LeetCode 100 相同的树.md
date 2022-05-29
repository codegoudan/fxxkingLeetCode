大家好呀，我是帅蛋。

今天解决**相同的树**，这是一道检验两棵二叉树是否相同的题目。

你如果看过我上篇关于【[**对称二叉树**](https://mp.weixin.qq.com/s/-ZRGSny3JEhAELp0yFQFJA)】的讲解，你就会发现在某种程度上这两道题可以看作是一道题。

那为什么我还要写这道题呢？

<div align=center>

![100-0](https://cdn.codegoudan.com/img/100-0.jpg)

</div>

对，我就是要看看小婊贝儿们到底有没有学会。

<div align=center>

![100-1](https://cdn.codegoudan.com/img/100-1.png)

</div>



# LeetCode 100：相同的树

题目链接：[相同的树](https://leetcode.cn/problems/same-tree/)



## 题意

给定两棵二叉树的根节点 p 和 q，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为他们是相同的。



## 示例

输入：p = [1,2,3]，q = [1,2,3]
输出：true

<div align=center>

![100-2](https://cdn.codegoudan.com/img/100-2.png)

</div>



## 提示

- 两棵树上的节点数目都在范围 [0,100] 内。
- -10^4 <= Node.val <= 10^4



# 题目解析

难度简单，似曾相识。

如果你已经看过【**[对称二叉树](https://mp.weixin.qq.com/s/-ZRGSny3JEhAELp0yFQFJA)**】这篇文章，那你看到这道题的时候，应该就已经知道怎么做了。

如果你没看过，那建议回过头儿去看一下，看完了，做这道题的感觉也就来了。

<div align=center>

![100-3](https://cdn.codegoudan.com/img/100-3.jpg)

</div>

我在【对称二叉树】中说过，对称二叉树就是看左右子树是否互相翻转，和根节点没什么关系。

既然没什么关系，那把根节点拿掉，其实就是在比较两棵树。

<div align=center>

![100-4](https://cdn.codegoudan.com/img/100-4.png)

</div>

那你看，从这个角度来看，这两道题其实就是一种类型的题，再直白点，就是一道题。

只不过稍微有点区别的是，对称二叉树是下面这种比较方式：

- 左子树的左子树和右子树的右子树比较。
- 左子树的右子树和右子树的左子树比较。

<div align=center>

![100-5](https://cdn.codegoudan.com/img/100-5.png)

</div>

而本题相同的树则是下面这种比较方式：

- p 树的根节点和 q 树的根节点比较。
- p 树的左子树和 q 树的左子树比较。
- p 树的右子树和 q 树的右子树比较。

<div align=center>

![100-6](https://cdn.codegoudan.com/img/100-6.png)

</div>

是不是就很一目了然？

<div align=center>

![100-7](https://cdn.codegoudan.com/img/100-7.jpg)

</div>

# 递归法

都是老套路。

<div align=center>

![100-8](https://cdn.codegoudan.com/img/100-8.jpg)

</div>

根据【[**递归算法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)】文章中讲的，实现递归，需要两步：

- 找出重复的子问题（递推公式）。
- 终止条件。

**(1) 找出重复的子问题。**

这个在上面看图的时候其实已经找出来了。

**p 树的左子树和 q 树的左子树，p 树的右子树和 q 树的右子树是否相等**。

如果相等就返回 true，不相等就返回 false。

```Python
# 判断两棵二叉树的左子树是否相同
leftTree = self.isSameTree(p.left, q.left)
# 判断两棵二叉树的右子树是否相同
rightTree = self.isSameTree(p.right, q.right)
```

**(2) 确定终止条件。**

同样，相同的树这道题因为比较的是节点的值是否相同，所以涉及的情况有点多。

首先是**根节点为空的情况（空树）**，根节点为空，分为 3 种情况：

- p，q 的根节点都为空，即为空树，显然两棵树相同。
- p 的根节点为空，q 的根节点不为空，显然两棵树不相同。
- p 的根节点不为空，q 的根节点为空，显然也是不相同的。

根节点为空的情况判断完了，剩下就是**根节点不为空的情况**，根节点不为空，其实终止就 1 种情况：

- 比较 p，q 根节点的值，如果不相等，则两棵树不相同。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:

        # 若两棵二叉树皆为空树，则两棵二叉树相同
        if p == None and q == None:
            return True
        # 如果其中一棵二叉树为空，另一棵不为空，则一定不相同
        if (p == None and q != None) or (p != None and q == None):
            return False
        # 如果两棵二叉树皆不为空，但是根节点的值不同，则一定不相同
        if p.val != q.val:
            return False
        # 判断两棵二叉树的左子树是否相同
        leftTree = self.isSameTree(p.left, q.left)
        # 判断两棵二叉树的右子树是否相同
        rightTree = self.isSameTree(p.right, q.right)

        isSame = leftTree and rightTree

        return isSame
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // 若两棵二叉树皆为空树，则两棵二叉树相同
        if(p == null && q == null){
            return true;
        }
        // 如果其中一棵二叉树为空，另一棵不为空，则一定不相同
        if((p == null && q != null) || (p != null && q == null)){
            return false;
        }
        // 如果两棵二叉树皆不为空，但是根节点的值不同，则一定不相同
        if(p.val !=q.val){
            return false;
        }
        // 判断两棵二叉树的左子树是否相同
        boolean leftTree = isSameTree(p.left, q.left);
        //  判断两棵二叉树的右子树是否相同
        boolean rightTree = isSameTree(p.right,q.right);

        boolean isSame = leftTree && rightTree;

        return isSame;
    }
}
```



假设 p 树有 n 个节点，q 树有 m 个节点。

此解法，不管是 p 和 q 树是否相同，都会将节点少的树的每个节点遍历一遍，所以**时间复杂度为 O(min(n,m))**。

使用递归，在过程中额外调用了栈空间，所以**空间复杂度为 O(min(n,m))**。



# 非递归法（迭代）

这道题其实就是对于每一层来说，**只要 p 树和 q 树的对应节点存在且相等即可**。

其实这还是类似于层次遍历，**使用队列来解决**。

**每次将 p 树和 q 树对应层的节点依次入队列，比如 p 的左节点和 q 的左节点入队列，p 的右节点和 q 的右节点入队列。**

比如对于下图：

<div align=center>

![100-9](https://cdn.codegoudan.com/img/100-9.png)

</div>

首先初始化队列，并将 p 树和 q 树的根节点入队列。

<div align=center>

![100-10](https://cdn.codegoudan.com/img/100-10.png)

</div>

```Python
# 初始化队列
queue = [p, q]
```



当队列不为空，将前两个元素出队列进行比较。

<div align=center>

![100-11](https://cdn.codegoudan.com/img/100-11.png)

</div>

```Python
# 从队列中取出两个节点
pNode = queue.pop(0)
qNode = queue.pop(0)
# 若当前节点为空，则继续循环
if pNode == None and qNode == None:
    continue
# 如果其中一个节点为空，另一个不为空，则一定不相同
if (pNode == None and qNode != None) or (pNode != None and qNode == None):
    return False
# 如果两个节点皆不为空，但是节点的值不同，则一定不相同
if pNode.val != qNode.val:
    return False
```



试下面再依次将 p 的左孩子和 q 的左孩子，p 的右孩子和 q 的右孩子入队列。

<div align=center>

![100-12](https://cdn.codegoudan.com/img/100-12.png)

</div>

接下来还是按照上面的方式出队列、比较、入队列...直至队列为空。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        # 初始化队列
        queue = [p, q]

        while queue:
            # 从队列中取出两个节点
            pNode = queue.pop(0)
            qNode = queue.pop(0)
            # 若当前节点为空，则继续循环
            if pNode == None and qNode == None:
                continue
            # 如果其中一个节点为空，另一个不为空，则一定不相同
            if (pNode == None and qNode != None) or (pNode != None and qNode == None):
                return False
            # 如果两个节点皆不为空，但是节点的值不同，则一定不相同
            if pNode.val != qNode.val:
                return False
            # pNode 节点的左孩子和 qNode 节点的左孩子入队列
            queue.append(pNode.left)
            queue.append(qNode.left)
            # pNode 节点的右孩子和 qNode 节点的右孩子入队列
            queue.append(pNode.right)
            queue.append(qNode.right)

        return True
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        // 初始化队列
        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(p);
        queue.add(q);

        while(queue.size() > 0) {
        // 从队列中取出两个节点
        TreeNode pNode = queue.removeFirst();
        TreeNode qNode = queue.removeFirst();
        // 若当前为空，则继续循环
        if(pNode == null && qNode == null) {
            continue;
        }
        // 如果其中一个节点为空，另一个不为空，则一定不相同
        if((pNode == null && qNode != null) || (pNode != null && qNode == null)){
            return false;
        }
        // 如果两个节点皆不为空，但是节点的值不同，则一定不相同
        if(pNode.val != qNode.val) {
            return false;
        }
        // pNode 节点的左孩子和 qNode 节点的左孩子入队列
        queue.add(pNode.left);
        queue.add(qNode.left);
        // pNode 节点的右孩子和 qNode 节点的右孩子入队列
        queue.add(pNode.right);
        queue.add(qNode.right);
        }
        return true;
    }
}
```



同样，非递归法，**时间复杂度为 O(min(n,m))，空间复杂度为 O(min(n,m))**。



---

**图解相同的树**到这就结束辣，你写对了么？

通过这道题，我其实想说，**很多时候你会发现，题目不过是在某类解决办法方面做加法减法**。

希望大家能多多思考，做题的时候不要做完了就算了。

当然文章也不能看完就算了呀，记得帮我**点赞**呀，么么哒。

我是帅蛋，我们下次见！
