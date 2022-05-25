大家好呀，我是帅蛋。

今天解决**树左下角的值**，和【[**左叶子之和**](https://mp.weixin.qq.com/s/1bP_WY4nx8pD22ASXnV6BA)】差不多，也是一个理解对了“左下角”是什么，就成功大部分的题。

话不多说，我们直接开始。

<div align=center>

![513-0](https://cdn.codegoudan.com/img/513-0.png)

</div>



# LeetCode 513：找树左下角的值



## 题意

给定一个二叉树的根节点 root，请找出该二叉树最底层最左边节点的值。



## 示例

输入：root = [1, 2, 3, 4, null, 5, 6, null, null, 7]
输出：7

<div align=center>

![513-1](https://cdn.codegoudan.com/img/513-1.png)

</div>



## 提示

二叉树中至少有一个节点

- 1 <= n <= 10^4
- -2^31 <= Node.val <= 2^31 - 1



# 题目解析

经典题目，难度中等。

首先这道题的重点，就是“树左下角的值”，题目中给到的解释是二叉树“最底层最左边节点的值”。

这就很明了了，首先是“最底层”，再就是“最左边”。

“最底层”也就是二叉树的最大深度，最大深度上的那一层一定是最后一层。

关于二叉树的最大深度，之前我曾经写过详细的文章：



[**ACM 选手图解 LeetCode 二叉树的最大深度**](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)



求二叉树的最大深度，我用了“自顶向下”、“自底向上”、“自左向右”三种方法。

根据这道题的自身还有“最左边”的特点，这里我给出 2 种解法：



**(1) 自顶向下**

自顶向下，就是从根节点递归到叶子节点。

对于每个节点来说，先判断是否为叶子节点。

如果不是叶子节点，因为要的是“最左边”的节点，所以每次先遍历左子树，再遍历右子树。

不知道大家看懂了没有，这种先判断节点、再左子树、最后右子树，说白了用的就是**前序遍历的方式**。



**(2) 自左向右**

自左向右，就是从根节点开始，一层一层的遍历二叉树。

很好理解，最后一层的第一个节点就是我们想要的值。

这种一层一层的遍历，其实用的就是**层次遍历**。如果你对层次遍历不是很熟悉，可以看我下面这篇文章：



[ACM 选手带你玩转二叉树层次遍历（递归 + 非递归）](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)



稍微改一下就过了。



# 递归法

递归法，我以自顶向下，即前序遍历的方式为例，解决本题。

既然是用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是按照往常，祭出递归二步曲：



**(1) 找出重复的子问题。**

前序遍历的顺序是：根、左子树、右子树。

在本题同样也是这个顺序：**判断节点、****递归左子树，递归右子树**。

对于左子树和右子树来说，也都是同样的操作。

这里我重点说一下判断节点。

在这题中我们其实要维护两个深度，一个是当前的遍历到的最大深度 maxDepth，另一个是当前节点所处的深度 leftDepth。

如果当前节点是叶子节点，且 leftDepth > maxDepth 的时候，更新 maxDepth 和当前的结果 res。

因为这证明**当前遍历的节点是新的一层最先被遍历的节点**，因为我们优先进行的是左子树的遍历，所以它肯定是当前层最左边的节点。

```Python
# 如果当前节点是叶子节点
if root.left == None and root.right == None:
    # 当前叶子节点的深度大于之前保存的最大深度
    # 证明是当前深度的最左边叶子节点，因为先递归左子树
    # 此时更新最大深度，更新结果值
    if leftDepth > self.maxDepth:
        self.maxDepth = leftDepth
        self.res = root.val

# 递归左子树
self.leftValue(root.left, leftDepth + 1)
# 递归右子树
self.leftValue(root.right, leftDepth + 1)
```



**(2) 确定终止条件。**

对于终止条件，即无可遍历的时候，直接返回即可。

```Python
if root == None:
    return
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
    def __init__(self):
        self.maxDepth = -1
        self.res = -1

    def leftValue(self, root: Optional[TreeNode], leftDepth):
        if root == None:
            return
        # 如果当前节点是叶子节点
        if root.left == None and root.right == None:
            # 当前叶子节点的深度大于之前保存的最大深度
            # 证明是当前深度的最左边叶子节点，因为先递归左子树
            # 此时更新最大深度，更新结果值
            if leftDepth > self.maxDepth:
                self.maxDepth = leftDepth
                self.res = root.val
        
        # 递归左子树
        self.leftValue(root.left, leftDepth + 1)
        # 递归右子树
        self.leftValue(root.right, leftDepth + 1)

    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        self.leftValue(root, 0)
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
    int maxDepth = -1, res = -1;

    private void leftValue(TreeNode root, int leftDepth){
        if(root == null){
            return ;
        }
        // 如果当前节点是叶子节点
        if(root.left == null && root.right == null){
            // 当前叶子节点的深度大于之前保存的最大深度
            // 证明是当前深度的最左边叶子节点，因为先递归左子树
            // 此时更新最大深度，更新结果值
            if(leftDepth > maxDepth){
                maxDepth = leftDepth;
                res = root.val;
            }
        }
        // 递归左子树
        leftValue(root.left, leftDepth + 1);
        // 递归右子树
        leftValue(root.right, leftDepth + 1);
    }

    public int findBottomLeftValue(TreeNode root) {
        leftValue(root, 0);
        return res;
    }
}
```



本题解，在递归过程中每个节点都被遍历到，**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

迭代法，我以自左向右，即层次遍历的方式为例。

我在【[**层次遍历**](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)】文章中说过，**非递归版的层次遍历用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

具体的做法就是：**使用队列保存每一层的节点，第 1 个出队列的节点值保存(即该层最左边的值)，****把队列里的所有节点出队列，然后把这些出去节点各自的子节点入队列**。用 depth 维护每一层。

比如对于下图：

<div align=center>

![513-2](https://cdn.codegoudan.com/img/513-2.png)

</div>

首先初始化队列、深度和结果，将根节点入队列。

<div align=center>

![513-3](https://cdn.codegoudan.com/img/513-3.png)

</div>

```Python
# 始化队列、层次及结果
queue = [root]
depth = 0
res = 0
```



当队列不为空，出队列，res 存储当前第 1 个出队列的节点值，将所有的子节点入队列，维护当前层。

<div align=center>

![513-4](https://cdn.codegoudan.com/img/513-4.png)

</div>

```Python
# 弹出当前层的所有节点，并将所有子节点入队列
for i in range(n):
    # 存储每一层的第一个元素（这样在最后一层最左边的元素就知道啦）
    if i == 0:
        res = queue[i].val
    node = queue.pop(0)
    if node.left:
        queue.append(node.left)
    if node.right:
        queue.append(node.right)
    depth += 1
```



下面就是按照上面的方式，出队列，记录每一层的第 1 个出队列的节点值，入队列，维护当前层，直至队列为空。

<div align=center>

![513-5](https://cdn.codegoudan.com/img/513-5.png)

![513-6](https://cdn.codegoudan.com/img/513-6.png)

![513-7](https://cdn.codegoudan.com/img/513-7.png)

![513-8](https://cdn.codegoudan.com/img/513-8.png)

![513-9](https://cdn.codegoudan.com/img/513-9.png)



![513-10](https://cdn.codegoudan.com/img/513-10.png)

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
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        # 始化队列、层次及结果
        queue = [root]
        depth = 0
        res = 0
        # 当队列不为空
        while queue:
            # 当前层的节点数
            n = len(queue)
            # 弹出当前层的所有节点，并将所有子节点入队列
            for i in range(n):
                # 存储每一层的第一个元素（这样在最后一层最左边的元素就知道啦）
                if i == 0:
                    res = queue[i].val
                node = queue.pop(0)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                
            depth += 1
        
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
    public int findBottomLeftValue(TreeNode root) {
        // 初始化队列、层次和结果
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;
        int res = 0;

        // 当队列不为空
        while(!queue.isEmpty()){
            // 当前层的节点数
            int n = queue.size();
            // 弹出当前层的所有节点，并将所有子节点入队列
            for(int i = 0; i < n; i++){
                TreeNode node = queue.poll();
                // 存储每一层的第一个元素（这样在最后一层最左边的元素就知道啦）
                if(i == 0){
                    res = node.val;
                }
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
            depth++;
        }
        return res;
    }
}
```



同样本题解**时间复杂度为 O(n)，空间复杂度为 O(n)**。



---

图解找树左下角的值到这就结束辣，你学废了嘛？

还是那句话：用二叉树的遍历去解题，是二叉树中很容易碰到的方法，大家还得多总结多思考。

今天就到这了，记得帮我**点赞**呀，爱你们~

我是帅蛋，我们下次见！
