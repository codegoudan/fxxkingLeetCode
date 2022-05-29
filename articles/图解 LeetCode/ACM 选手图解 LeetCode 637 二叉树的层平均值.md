大家好呀，我是帅蛋。

今天解决**二叉树的层平均值**，是一道二叉树层次遍历的变形题目，难度不大，今天让我们一起来解决掉它！

<div align=center>

![637-0](https://cdn.codegoudan.com/img/637-0.png)

</div>



# LeetCode 637：二叉树的层平均值

题目链接：[二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)



## 题意

给定一个非空二叉树的根节点 root，以数组的形式返回每一层节点的平均值。



## 示例

输入：root = [3,9,20,null,null,15,7]
输出：[3.00000,14.50000,11.00000]解释：第 0 层的平均值为 3，第 1 层的平均值为 14.5，第 2 层的平均值为 11。

<div align=center>

![637-1](https://cdn.codegoudan.com/img/637-1.png)

</div>



## 提示

- 树中节点数量在 [1,10^4] 范围内
- -2^31 <= Node.val <= 2^31 - 1



# 题目解析

这道二叉树的层平均值，难度简单。

二叉树的层次遍历不太懂的可以看下面这篇文章：



[ACM 选手带你玩转二叉树层次遍历（递归 + 非递归）](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)



鉴于二叉树层次遍历相关题目难度忽简单忽中等，真的很迷，这道题定位难度简单，我也只能归于题目里带了一个“层”字。

大概心理活动就是：我都告诉解法就是层次遍历了，你还要我怎样？

<div align=center>

![637-8](https://cdn.codegoudan.com/img/637-8.jpg)

</div>

那这道题的具体做法就是：**遍历二叉树的每一层，记录该层的节点总值和节点数，然后求该层平均值加入 res 结果集中，直至遍历完整棵二叉树。**

至于每层的遍历，我们使用队列这种数据结构。

队列是一种**先进先出**（First in First Out）的数据结构，简称 **FIFO**。

如果不太了解队列的，可以看下面这篇文章：



**[ACM 选手带你玩转栈和队列](https://mp.weixin.qq.com/s/4D0FQiJMJ9wA4_ejkGbwiA)**



**使用队列保存每一层的所有节点，把队列里的所有节点出队列，然后把这些出去节点各自的子节点入队列**。



# 图解

以 root = [3,9,20,null,null,15,7] 为例：

<div align=center>

![637-2](https://cdn.codegoudan.com/img/637-2.png)

</div>

首先初始化队列和结果集，将根节点入队列。

<div align=center>

![637-3](https://cdn.codegoudan.com/img/637-3.png)

</div>

```Python
res = []
queue = [root]
```

第 1 步：队列不为空，初始化存储当前层孩子节点的列表 chilldNodes、当前层节点和 sum。

```Python
# 存储当前层的孩子节点列表
childNodes = []
# 存储当前层的节点数
cnt = len(queue)
# 存储当前层节点和
sum = 0
```

此时 queue 中的节点个数 cnt = 1，对应的节点值为 3，所以当前层的节点和 sum = 3。

此时将第 1 层的层平均值为 3/1 = 3，将此值加入结果集 res。

<div align=center>

![637-4](https://cdn.codegoudan.com/img/637-4.png)

</div>

```Python
# 求当前层节点和
for node in queue:
    sum += node.val
res.append(sum/cnt)
```

将队列中节点的左孩子和右孩子依次入队列 childNodes。

<div align=center>

![637-5](https://cdn.codegoudan.com/img/637-5.png)

</div>

```Python
for node in queue:
    # 若节点存在左孩子，入队
    if node.left:
        childNodes.append(node.left)
    # 若节点存在右孩子，入队
    if node.right:
        childNodes.append(node.right)
```

将当前队列 queue 更新为 childNodes，继续进行下一层的遍历。

<div align=center>

![637-6](https://cdn.codegoudan.com/img/637-6.png)

</div>

```Python
# 更新队列为下一层的节点，继续遍历
queue = childNodes
```

之后就是重复上面的操作，直至全部遍历完。

<div align=center>

![637-7](https://cdn.codegoudan.com/img/637-7.png)

</div>



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
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        if root == None:
            return []

        res = []
        queue = [root]

        while queue:
            # 存储当前层的孩子节点列表
            childNodes = []
            # 存储当前层的节点数
            cnt = len(queue)
            # 存储当前层节点和
            sum = 0
            # 求当前层节点和
            for node in queue:
                sum += node.val
            res.append(sum/cnt)

            for node in queue:
                # 若节点存在左孩子，入队
                if node.left:
                    childNodes.append(node.left)
                # 若节点存在右孩子，入队
                if node.right:
                    childNodes.append(node.right)
            # 更新队列为下一层的节点，继续遍历
            queue = childNodes

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
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<Double>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            // 存储当前层节点和
            double sum = 0;
            // 存储当前层的节点数
            int cnt = queue.size();
            for (int i = 0; i < cnt; i++) {
                TreeNode node = queue.poll();
                // 求当前层节点和
                sum += node.val;
                // 若节点存在左孩子，入队
                if (node.left != null) {
                    queue.offer(node.left);
                }
                // 若节点存在右孩子，入队
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            res.add(sum / cnt);
        }
        return res;
    }
}
```



本题解，对于每个节点，各进出队列一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列和保存当前层孩子节点的列表，所以**空间复杂度为 O(n)**。



---

**图解二叉树的层平均值**到这就结束辣，我现在真的是太爱层次遍历了，真好用！

快把爱他打在公屏上！

<div align=center>

![637-9](https://cdn.codegoudan.com/img/637-9.jpg)

</div>

今天就到这辣，我是帅蛋，我们下次见~
