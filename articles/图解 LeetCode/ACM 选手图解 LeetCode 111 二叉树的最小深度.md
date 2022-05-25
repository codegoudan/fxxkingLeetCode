大家好，我是帅蛋。

之前的文章解决了[**二叉树的最大深度**](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)，不止于此，劳模蛋甚至连 [**N 叉树的最大深度**](https://mp.weixin.qq.com/s/JcNYsiQ5xsBiX69vgEWWuQ)也一并解决了，从此叉多叉少不再是求树最大深度的障碍。

有大就有小，最大解决了，那最小怎么搞呢？

不慌，今天呀，就让我们一起来解决**二叉树最小深度**的问题，开整！

<div align=center>

![111-0](https://cdn.codegoudan.com/img/111-0.png)

</div>



# LeetCode 111：二叉树的最小深度



## 题意

给定一个二叉树，找出其最小深度。

最小深度是从根节点到叶子节点的最短路径上的节点数量。



## 示例

输入：root = [3,9,20,null,null,15,7]
输出：2

<div align=center>

![111-1](https://cdn.codegoudan.com/img/111-1.png)

</div>



## 提示

- 树中节点数的范围在 [0, 10^5] 之间。
- -1000 <= Node.val <= 1000。



# 题目解析

难度简单，是求二叉树深度的经典题目。

我们在之前已经做过求二叉树最大深度：



[ACM 选手图解 LeetCode 二叉树的最大深度](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)



别看求二叉树的最小深度，只是和求二叉树的最大深度差了 1 个字，在解法上，却不只是将“求最大改为求最小”那么简单。

就不卖关子了，求二叉树的最小深度主要有 2 种常规解法：

**(1) 自底向上**

自底向上，从叶子节点开始，一层一层的向上，最终汇集在根节点。

每次先遍历左子树，找出左子树的最小深度，再遍历右子树，找出右子树的最小深度，最终再根节点取左子树和右子树深度最小的那个，加上根节点的高度 1，即 min(leftMindepth, rightMindepth) + 1 为当前二叉树的最小深度。

同样可以看出，这种先递归左子树，再递归右子树，最后再是根节点，用的还是后序遍历。

<div align=center>

![111-2](https://cdn.codegoudan.com/img/111-2.png)

</div>



**(2) 自左向右**

自左向右，就是从根节点开始，一层一层的遍历二叉树。

这种一层一层的遍历，其实用的就是之前讲过的层次遍历。

你从第一层往下，一层一层的看，会发现一个很有意思的事，当发现第 1 个左右节点都为空的节点（即叶子节点）的时候，它的深度就是二叉树的最小深度。

大家多画几个不一样的二叉树验证一下。

<div align=center>

![111-3](https://cdn.codegoudan.com/img/111-3.png)

</div>

# 递归法

递归法，我以自底向上方式为例。

既然是用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是按照往常，祭出递归二步曲：

**(1) 找出重复的子问题。**

后序遍历的顺序是：左子树、右子树、根。

在本题同样也是这个顺序：**递归左子树的最小高度，递归右子树的最小高度，求根的最小高度**。

对于左子树和右子树来说，也都是同样的操作。

```Python
# 左子树最小值和右子树最小值
leftMindepth = self.minDepth(root.left)
rightMindepth = self.minDepth(root.right)
```



求二叉树的最小深度也就成了：

```Python
min(leftMindepth, rightMindepth) + 1
```



也就是下图这样：

<div align=center>

![111-4](https://cdn.codegoudan.com/img/111-4.png)

</div>



**如果你到这就结束了，那很遗憾的告诉你，这道题你已经完蛋了...**

我们再来看题，题目中定义“最小深度是从根节点到最近叶子节点的最短路径上的节点数量”。

我们都知道叶子节点是指没有左右孩子的节点。如果你只做到了上面那样的求解，那像下面这种二叉树：

<div align=center>

![111-5](https://cdn.codegoudan.com/img/111-5.png)

</div>



你得到的结果会是：minDepth = min(leftMindepth, rightMindepth) + 1 = min(1, 0) + 1 = 1。

这个结果显然是错误的。这样的解法是把根节点 1 当成了叶子节点！而根节点 1 有左孩子，它不是叶子节点！

**这就是这道题和之前求二叉树的最大深度不同的地方，也是这道题最大的坑！稍不注意，就完犊子！**

因为再三强调：最小深度是从根节点到最近叶子节点的最短路径上的节点数量。**上面的二叉树只有左子树才有叶子节点，而右子树是空的，没有叶子节点，也就不存在从根节点到右子树的最小深度。**

所以它的最小深度应该是 2。

**所以，对于重复子问题应该分情况讨论：**

**第 1 种情况，只有根节点的**，最小深度为 1。

```Python
# 只有根节点，最小高度为 1
if root.left == None and root.right == None:
    return 1
```



**第 2 种情况，只有左子树或者只有右子树的**，最小深度为“左子树的最小深度 + 1” 或者“右子树的最小深度 + 1”。

```Python
# 如果节点的左子树不为空，右子树为空
if root.left != None and root.right == None:
    return leftMindepth + 1
# 如果节点的右子树不为空，左子树为空
if root.left == None and root.right != None:
    return rightMindepth + 1
```



**第 3 种情况，左子树和右子树都有的**，最小深度为“min(左子树的最小深度，右子树的最小深度) + 1”

```Python
# 左右子树都不为空
return min(leftMindepth, rightMindepth) + 1
```



**(2) 确定终止条件。**

还是一样的，对于二叉树的遍历来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

那就是当前的节点是空的，既然是空的那就没啥好遍历。

```Python
# 二叉树为空，最小高度为 0
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
    def minDepth(self, root: TreeNode) -> int:
        # 二叉树为空，最小高度为 0
        if root == None:
            return 0
        # 只有根节点，最小高度为 1
        if root.left == None and root.right == None:
            return 1
        # 左子树最小值和右子树最小值
        leftMindepth = self.minDepth(root.left)
        rightMindepth = self.minDepth(root.right)

        # 如果节点的左子树不为空，右子树为空
        if root.left != None and root.right == None:
            return leftMindepth + 1
        # 如果节点的右子树不为空，左子树为空
        if root.left == None and root.right != None:
            return rightMindepth + 1
        # 左右子树都不为空
        return min(leftMindepth, rightMindepth) + 1
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
    public int minDepth(TreeNode root) {
        // 二叉树为空，最小高度为 0
        if(root == null){
            return 0;
        }
        // 只有根节点，最小高度为 1
        if (root.left == null && root.right == null){
            return 1;
        }
        // 左子树最小值和右子树最小值
        int leftMindepth = minDepth(root.left);
        int rightMindepth = minDepth(root.right);
        // 如果节点的左子树不为空，右子树为空
        if(root.left != null && root.right == null){
            return leftMindepth + 1;
        }
        // 如果节点的右子树不为空，左子树为空
        if(root.left == null && root.right != null){
            return rightMindepth + 1;
        }
        // 左右子树都不为空
        return Math.min(leftMindepth, rightMindepth) + 1;
    }
}
```



本题解，在递归过程中对每个节点都访问 1 次，**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

迭代法，我以自左向右，即层次遍历的方式为例。

我在【[**ACM 选手图解 LeetCode N 叉树的层序遍历**](https://mp.weixin.qq.com/s/uAiSgU1I4Q57dTaTHKbgpg)】文章中说过，**非递归版的层次遍历用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

具体做法就是：**使用队列保存每一层的所有节点，把队列里的所有节点依次出队列，当出队列的节点无左右孩子（即叶子节点），立即返回当前层数（即为最小深度），否则把这些出去节点各自的子节点入队列**。用 depth 维护每一层。

比如对于下图：

<div align=center>

![111-6](https://cdn.codegoudan.com/img/111-6.png)

</div>

首先初始化队列 queue 和层次 depth，将根节点入队列：

<div align=center>

![111-7](https://cdn.codegoudan.com/img/111-7.png)

</div>

```Python
# 初始化队列和层次
queue = [root]
depth = 1
```

当队列不为空，出队列，将所有的子节点入队列。

<div align=center>

![111-8](https://cdn.codegoudan.com/img/111-8.png)

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



下面就是按照上面的方式，出队列，**判断当前出队列的节点是否为叶子节点**，如果是，则返回结果，否则继续出队列，入队列，维护当前层次，直至队列为空。

<div align=center>

![111-9](https://cdn.codegoudan.com/img/111-9.png)

</div>

此时 9 为叶子节点，直接返回当前结果，即二叉树的最小深度为 2。

```Python
# 最早出现左右节点都为空，证明为叶子节点，即为二叉树的最小高度
if node.left == None and node.right == None:
    return depth
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
    def minDepth(self, root: TreeNode) -> int:
        # 空树，深度为 0
        if root == None:
            return 0
        # 初始化队列和层次
        queue = [root]
        depth = 1
        # 当队列不为空
        while queue:
            # 当前层的节点数
            n = len(queue)
            # 弹出当前层的所有节点，并将所有子节点入队列
            for i in range(n):
                node = queue.pop(0)
                # 最早出现左右节点都为空，证明为叶子节点，即为二叉树的最小高度
                if node.left == None and node.right == None:
                    return depth
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            depth += 1

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
    public int minDepth(TreeNode root) {
        // 空树，高度为 0
        if(root == null){
            return 0;
        }
        // 初始化队列和层次
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 1;

        // 当队列不为空
        while(!queue.isEmpty()){
            // 当前层的节点数
            int n = queue.size();
            // 弹出当前层的所有节点，并将所有子节点入队列
            for(int i = 0; i < n; i++){
                TreeNode node = queue.poll();
                // 最早出现左右节点都为空，证明为叶子节点，即为二叉树的最小高度
                if(node.left == null && node.right == null){
                    return depth;
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

        return depth;
    }
}
```



本题解，对于每个节点，各进出队列一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列，所以**空间复杂度为 O(n)**。



---

到这，**图解二叉树的最小深度**就结束辣，咋样，是不是收获满满？

你再看，搞个二叉树的最小深度也能和二叉树的遍历搞上关系。所以，还是我一直说的那句话，**题目的解决都是从我们过去学过的知识中寻找办法**。

今天就到这了，记得帮我**点赞**呀，爱你们~

我是帅蛋，我们下次见！
