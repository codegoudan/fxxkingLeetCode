大家好呀，我是快乐的蛋蛋。

今天解决**二叉树的右视图**，右视图说白了就是每一层最右面的节点，那我们怎么把它们找出来呢？

这就是今天要解决的问题，咱们话不多说，直接开始~

<div align=center>

![199-0](https://cdn.codegoudan.com/img/199-0.png)

</div>



# LeetCode 199：二叉树的右视图



## 题意

给定一个二叉树的根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。



## 示例

输入：[1,2,3,null,5,null,4]
输出：[1,3,4]

<div align=center>

![199-1](https://cdn.codegoudan.com/img/199-1.png)

</div>



## 提示

- 二叉树的节点个数的范围是 [0,100]
- -100 <= Node.val <= 100



# 题目解析

这道二叉树的右视图官方定级难度中等，说实话如果你是一直跟着我的题解走下来的，对你来说就是道简单题。

这个所谓的右视图，其实就是每一层最右边的那个节点，涉及到“层”的概念，我们就可以直接祭出层次遍历来解决。

如果对层次遍历不太了解的同学，可以看我下面这篇文章：



**[ACM 选手带你玩转二叉树层次遍历（递归 + 非递归）](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)**



做法也很简单，对于每一层的节点从左到右遍历，保存每层最后一个遍历的节点即可。

<div align=center>

![199-2](https://cdn.codegoudan.com/img/199-2.png)

</div>



# 递归法

用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是按照往常，祭出递归二步曲：



**(1) 找出重复的子问题。**

层次遍历对于每一层，我们都是从左到右遍历，这次我们换个方向，从右向左遍历。

这样对于每一层来说，遍历的第一个节点就是。

所以**在遍历的时候我们可以先遍历右子树，再遍历左子树**。

```Python
# 如果当前节点的深度还没出现在 res 中（因为一层就一个节点）
# 那说明当前节点是该层第一个被访问的节点，将当前节点的值加入 res
if len(res) < depth:
    res.append(root.val)
# 遍历右子树
if root.right:
    self.level(root.right, depth + 1, res)
# 遍历左子树
if root.left:
    self.level(root.left, depth + 1, res)
```



**(2) 确定终止条件。**

对于二叉树的遍历来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

即最下面一层的左右节点都为空了。

```Python
if root == None:
    return []
```



最重要的两点确定好了，代码也就出来了。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def level(self, root:TreeNode, depth, res):
        if root == None:
            return []
        # 如果当前节点的深度还没出现在 res 中（因为一层就一个节点）
        # 那说明当前节点是该层第一个被访问的节点，将当前节点的值加入 res
        if len(res) < depth:
            res.append(root.val)
        # 遍历右子树
        if root.right:
            self.level(root.right, depth + 1, res)
        # 遍历左子树
        if root.left:
            self.level(root.left, depth + 1, res)

    def rightSideView(self, root: TreeNode) -> List[int]:
        res = []
        self.level(root, 1, res)
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

    void level(TreeNode root, int depth, List<Integer> res){
        // 如果当前节点的深度还没出现在 res 中（因为一层就一个节点）
        // 那说明当前节点是该层第一个被访问的节点，将当前节点的值加入 res
        if(res.size() < depth){
            res.add(root.val);
        }
        // 遍历右子树
        if(root.right != null){
            level(root.right, depth + 1, res);
        }
        // 遍历左子树
        if(root.left != null){
            level(root.left, depth + 1, res);
        }
    }

    public List<Integer> rightSideView(TreeNode root) {
        if(root == null){
            return new ArrayList<Integer>();
        }
        List<Integer> res = new ArrayList<Integer>();
        level(root, 1, res);
        return res;
    }
}
```



本题解，在递归过程中每个节点都被遍历到，**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

**非递归版的层次遍历用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

具体做法就是：**使用队列保存每一层的所有节点，把队列里的所有节点出队列，如果当前节点是当前层的最后一个，入 res。**

比如对于下图：

<div align=center>

![199-3](https://cdn.codegoudan.com/img/199-3.png)

</div>

首先初始化队列和结果 res，将根节点入队列。

<div align=center>

![199-4](https://cdn.codegoudan.com/img/199-4.png)

</div>

```Python
# 初始化队列和结果
queue = [root]
res = []
```



当队列不为空，节点 1 出队列，此时 node = 1 是当前层的最后一个，将节点值加入 res。

<div align=center>

![199-5](https://cdn.codegoudan.com/img/199-5.png)

</div>

```Python
# 如果节点是当前层的最后一个，则入 res
if(i == size - 1):
    res.append(node.val)
```



同时将 node = 1 的子节点入队列 queue。

<div align=center>

![199-6](https://cdn.codegoudan.com/img/199-6.png)

</div>

```python
// 将当前层的所有节点入队列
if(node.left != null){
    queue.offer(node.left);
}
if(node.right != null){
    queue.offer(node.right);
}
```



之后就是重复上面的操作，不断的入队列出队列，直至队列为空。

<div align=center>

![199-7](https://cdn.codegoudan.com/img/199-7.png)

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
    def rightSideView(self, root: TreeNode) -> List[int]:
        if root == None:
            return []
        # 初始化队列和结果
        queue = [root]
        res = []
        # 当队列不为空
        while queue:
            # 当前层的节点数
            size = len(queue)
            # 弹出当前层的所有节点
            for i in range(size):
                node = queue.pop(0)
                # 如果节点是当前层的最后一个，则入 res
                if(i == size - 1):
                    res.append(node.val)
                # 将当前层的所有节点入队列
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return res
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
    public List<Integer> rightSideView(TreeNode root) {
        if(root == null){
            return new ArrayList<Integer>();
        }
        // 初始化队列和结果
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        List<Integer> res = new ArrayList<Integer>();
        // 当队列不为空
        while(!queue.isEmpty()){
            // 当前层的节点数
            int size = queue.size();
            // 弹出当前层的所有节点
            for(int i = 0; i < size; i++){
                TreeNode node = queue.poll();
                // 如果节点是当前层的最后一个，则入 res
                if(i == size - 1){
                    res.add(node.val);
                }
                // 将当前层的所有节点入队列
                if(node.left != null){
                    queue.offer(node.left);
                }
                if(node.right != null){
                    queue.offer(node.right);
                }
            }
        }
        return res;
    }
}
```



本题解，对于每个节点，各进出队列一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列，所以**空间复杂度为 O(n)**。



---

**图解二叉树的右视图**到这就结束辣，层次遍历 YYDS，都给我吹起来！

当然还要记得做好总结呦~

今天就到这了，记得帮我**点赞**呀，爱你们~

我是帅蛋，我们下次见！
