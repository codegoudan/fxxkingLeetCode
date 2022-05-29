大家好呀，我是深水炸蛋。

今天解决**二叉树的最大深度**，二叉树的经典好题。

二叉树的题目，做多了你就会发现，题目之间都存在相似性，解题是有固定的套路。

下面让我们一起来看题吧！

<div align=center>

![104-1](https://cdn.codegoudan.com/img/104-1.png)

</div>



# LeetCode 104：二叉树的最大深度

题目链接：[二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)



## 题意

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。



## 示例

输入：[3,9,20,null,null,15,7]
输出：3

<div align=center>

![104-2](https://cdn.codegoudan.com/img/104-2.png)

</div>



## 提示

- 爱帅蛋
- 么么哒



# 题目解析

经典题目，难度简单。

**解决这道题的重点，在我看来，其实是考察你对二叉树的概念理解是否到位。**

懂了概念，这道题的解法也就出来了，而且是不止一个的解法。

这道题是求二叉树的最大深度，**二叉树的深度是从根节点开始算起，依次往下是深度 1、2、...**

可以**理解成一口井，从上往下看，也就是自顶向下看**。

我们来看下面这张图：

<div align=center>

![104-3](https://cdn.codegoudan.com/img/104-3.png)

</div>

这张图中有 3 个树的重要概念：层次、深度、高度。

**二叉树的层次是从根节点算起，根节点是第一层，依次往下类推。**

**二叉树的高度是从叶子节点算起，叶子节点高度是 1，依次往上类推**。**可以看成是高楼，从下往上看，也就是自底向上看**。

通过图，也可以看出，**二叉树的深度和层次是完全对应的，最大深度为最大层次数。二叉树的深度和高度正好相反**。

了解了这些，你会发现根据看的顺序不同，这道题的 3 种常规解法：



**(1) 自顶向下**

自顶向下，就是从根节点递归到叶子节点，计算这一条路径上的深度，并更新维护最大深度。

这个是正儿八经的求深度。**每次先维护根节点的深度，再递归左子树、右子树**。

不知道大家看懂了没，每次都是先根节点，再是左子树，最后右子树，说白了用的其实就**是前序遍历的方式**。

<div align=center>

![104-4](https://cdn.codegoudan.com/img/104-4.png)

</div>



**(2) 自底向上**

自底向上，从叶子节点开始，一层一层的向上，最终汇集在根节点。

**这种求的其实是二叉树的高度**。先遍历左子树，找出最大高度，再遍历右子树找出最大高度，最后在根节点取左子树和右子树高度值大的那个，加上根节点的高度 1，即 max(leftHeight, rightHeight) + 1 为当前二叉树的最大高度。

因为**二叉树的最大高度 = 最大深度**，所以即可求出二叉树的最大深度。

可以看出，这种先递归左子树、再递归右子树，最后再根节点，用的其实是**后序遍历**。

<div align=center>

![104-5](https://cdn.codegoudan.com/img/104-5.png)

</div>



**(3) 自左向右**

自左向右，就是从根节点开始，一层一层的遍历二叉树。

**这种求的是二叉树的层****次。二叉树的最大层次就是其最大深度。**

可以看出，这种一层一层的遍历，其实用的就是**层次遍历**。

<div align=center>

![104-6](https://cdn.codegoudan.com/img/104-6.png)

</div>



# 递归

递归法，我以自底向上，即后序遍历的方式为例，解决本题，因为这相比而言，更好理解。

既然是用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是按照往常，祭出递归二步曲：

**(1) 找出重复的子问题。**

后序遍历的顺序是：左子树、右子树、根。

在本题同样也是这个顺序：**递归左子树的最大高度，递归右子树的最大高度，求根的最大高度**。

对于左子树和右子树来说，也都是同样的操作。

```Python
# 递归计算左子树的最大深度
leftHeight = self.maxDepth(root.left)
# 递归计算右子树的最大深度
rightHeight = self.maxDepth(root.right)
# 二叉树的最大深度 = 子树的最大深度 + 1（1 是根节点）
maxHeight = max(leftHeight, rightHeight) + 1
```



可能我把图换成这样更好理解：

<div align=center>

![104-7](https://cdn.codegoudan.com/img/104-7.png)

</div>

**(2) 确定终止条件。**

对于二叉树的遍历来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

那就是当前的节点是空的，既然是空的那就没啥好遍历。

```Python
# 节点为空，高度为 0
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
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # 节点为空，高度为 0
        if root == None:
            return 0

        # 递归计算左子树的最大深度
        leftHeight = self.maxDepth(root.left)
        # 递归计算右子树的最大深度
        rightHeight = self.maxDepth(root.right)

        # 二叉树的最大深度 = 子树的最大深度 + 1（1 是根节点）
        return max(leftHeight, rightHeight) + 1
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
    public int maxDepth(TreeNode root) {
        // 节点为空，高度为 0
        if(root == null){
            return 0;
        }
        // 递归计算左子树的最大深度
        int leftHeight = maxDepth(root.left);
        // 递归计算右子树的最大深度
        int rightHeight = maxDepth(root.right);
        // 二叉树的最大深度 = 子树的最大深度 + 1（1 是根节点）
        return Math.max(leftHeight, rightHeight) + 1;
    }
}
```



本题解，在递归过程中每个节点都被遍历到，**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

迭代法，我以自左向右，即层次遍历的方式为例。

我在【[**层次遍历**](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)】文章中说过，**非递归版的层次遍历用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

具体做法就是：**使用队列保存每一层的所有节点，把队列里的所有节点出队列，然后把这些出去节点各自的子节点入队列**。用 depth 维护每一层。

比如对于下图：

<div align=center>

![104-8](https://cdn.codegoudan.com/img/104-8.png)

</div>

首先初始化队列 queue 和层次 depth，将根节点入队列：

<div align=center>

![104-9](https://cdn.codegoudan.com/img/104-9.png)

</div>

```Python
# 初始化队列和层次
queue = [root]
depth = 0
```

当队列不为空，出队列，将所有的子节点入队列。

<div align=center>

![104-10](https://cdn.codegoudan.com/img/104-10.png)

</div>

```Python
# 当队列不为空
while queue:
    # 当前层的节点数
    n = len(queue)
    # 弹出当前层的所有节点，并将所有子节点入队列
    for i in range(n):
        node = queue.pop(0)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    depth += 1
```

下面就是按照上面的方式，出队列，入队列，维护当前层次，直至队列为空。

<div align=center>

![104-11](https://cdn.codegoudan.com/img/104-11.png)

![104-12](https://cdn.codegoudan.com/img/104-12.png)

![104-13](https://cdn.codegoudan.com/img/104-13.png)

![104-14](https://cdn.codegoudan.com/img/104-14.png)

</div>

## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # 空树，高度为 0
        if root == None:
            return 0

        # 初始化队列和层次
        queue = [root]
        depth = 0

        # 当队列不为空
        while queue:
            # 当前层的节点数
            n = len(queue)
            # 弹出当前层的所有节点，并将所有子节点入队列
            for i in range(n):
                node = queue.pop(0)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            depth += 1
        # 二叉树最大层次即为二叉树最深深度
        return depth
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
    public int maxDepth(TreeNode root) {
        // 空树，高度为 0
        if(root == null){
            return 0;
        }
        // 初始化队列和层次
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;

        // 当队列不为空
        while(!queue.isEmpty()){
            // 当前层的节点数
            int n = queue.size();
            // 弹出当前层的所有节点，并将所有子节点入队列
            for(int i = 0; i < n; i++){
                TreeNode node = queue.poll();
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
            depth++;
        }
        // 二叉树最大层次即为二叉树最深深度
        return depth;
    }
}
```



本题解，对于每个节点，各进出队列一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列，所以**空间复杂度为 O(n)**。



---

**图解二叉树的最大深度**到这就结束辣，做二叉树的题是不是有点感觉惹？

你看，搞个二叉树的最大深度又和遍历扯上了关系，有没有突然想起【[**翻转二叉树**](https://mp.weixin.qq.com/s/HHHcFsLlsz9prghr42fSWg)】这道题？

你看吧，还是那句话，**题目的解决都是从我们过去学过的知识中寻找办法**。

今天就这了，记得帮我**点赞 **呀，爱你们~

我是帅蛋，我们下次见！
