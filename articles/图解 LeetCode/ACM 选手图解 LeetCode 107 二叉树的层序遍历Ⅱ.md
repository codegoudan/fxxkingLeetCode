大家好呀，我是帅蛋。

今天解决**二叉树的层序遍历Ⅱ**，我们通过这道题继续来练习层序遍历。

我之前说过，二叉树的层序遍历历来是面试的高频问题，而且通过前面二叉树的题目我们也可以发现”层序遍历真的能解决大部分常见的二叉树题目“。

一定要好好重视，下面我们来直接开整！

<div align=center>

![107-0](https://cdn.codegoudan.com/img/107-0.png)

</div>



# LeetCode 107：二叉树的层序遍历Ⅱ



## 题意

给你二叉树的根节点 root，返回其节点值自底向上的层序遍历。

即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历。



## 示例

输入：root = [3,9,20,null,null,15,7]
输出：[[15,7],[9,20],[3]]

<div align=center>

![107-1](https://cdn.codegoudan.com/img/107-1.png)

</div>



## 提示

- 树中节点数目在范围 [0,2000] 内
- -1000 <= Node.val <= 1000



# 题目解析

二叉树的层序遍历，我们先来理解它的涵义。

**层次遍历就是表面意思，一层层的遍历，同一层的遍历按照从左到右逐个遍历。**

<div align=center>

![107-2](https://cdn.codegoudan.com/img/107-2.png)

</div>

像上面这个二叉树，它的层次遍历顺序为：ABCDEFGHIJ。

这是正常的层序遍历，而二叉树的层序遍历Ⅱ这道题要求从下往上遍历，这个怎么整呢？

很简单，**按照正常层序遍历，完事以后，再把结果逆序输出。**

<div align=center>

![107-3](https://cdn.codegoudan.com/img/107-3.jpg)

</div>



# 递归法

用[**递归法**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475925238&idx=1&sn=ea7eafee3e61b433642312f33a5a2413&chksm=ff22f97bc855706d385c42fbb5132da5c7a2a53a2ca59e6a475f7ccfb3c2dc69195f6716e58a&scene=21#wechat_redirect)，那还是按照往常，祭出递归二步曲：

**(1) 找出重复的子问题。**

层次遍历是每一层的节点从左到右的遍历，所以**在遍历的时候我们可以先遍历左子树，再遍历右子树**。

<div align=center>

![107-4](https://cdn.codegoudan.com/img/107-4.png)

</div>

可能换成下面这样更清楚点：

<div align=center>

![107-5](https://cdn.codegoudan.com/img/107-5.png)

</div>

遍历的顺序是：3 -> 9 -> 3 -> 20 -> 15 -> 20 -> 7。

需要注意的是，在遍历左子树或者右子树的时候，涉及到向上或者向下遍历，为了让递归的过程中的同一层的节点放在同一个列表中，**在递归时要记录深度 depth**。

同时，**每次遍历到一个新的 depth，结果数组中没有对应的 depth 的列表时，在结果数组中创建一个新的列表保存该 depth 的节点**。

```Python
# 若当前行对应的列表不存在，加一个空列表
if len(res) < depth:
    res.append([])
```



**(2) 确定终止条件。**

对于二叉树的遍历来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

即最下面一层的左右节点都为空了。

```Python
if root == None:
    return []
```



最后记得输出结果逆序一下，代码就出来啦！



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def level(self, root: TreeNode, depth, res):
        if root == None:
            return []

        # 若当前行对应的列表不存在，加一个空列表
        if len(res) < depth:
            res.append([])

        # 将当前节点的值加入当前行的 res 中
        res[depth - 1].append(root.val)

        # 递归处理左子树
        if root.left:
            self.level(root.left, depth + 1, res)
        # 递归处理右子树
        if root.right:
            self.level(root.right, depth + 1, res)

    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        res = []
        self.level(root, 1, res)
        return res[::-1]
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
    void level(TreeNode root, int index, List<List<Integer>> res) {
        // 当前行对应的列表不存在，加一个空列表
        if(res.size() < index) {
            res.add(new ArrayList<Integer>());
        }
        // 将当前节点的值加入当前行的 res 中
        res.get(index-1).add(root.val);
        // 递归处理左子树
        if(root.left != null) {
            level(root.left, index+1, res);
        }
        // 递归处理右子树
        if(root.right != null) {
            level(root.right, index+1, res);
        }
  }
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        if(root == null) {
            return new ArrayList<List<Integer>>();
        }

        List<List<Integer>> list = new ArrayList<List<Integer>>();
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        level(root, 1, list);
        for(int i = list.size()-1; i >= 0; i--){
            res.add(list.get(i));
        }
        return res;
    }
}
```



二叉树层次遍历递归版，由于每个节点都被遍历到，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，维护了一个 res 的结果数组，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

**非递归版的层次遍历我们用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

队列是一种**先进先出**（First in First Out）的数据结构，简称 **FIFO**。

如果不太了解队列的，可以看下面这篇文章：



**[ACM 选手带你玩转栈和队列](https://mp.weixin.qq.com/s/4D0FQiJMJ9wA4_ejkGbwiA)**



思路很简单：

**使用队列保存每一层的所有节点，把队列里的所有节点出队列，然后把这些出去节点各自的子节点入队列**。

以此来完成对每层的遍历。

<div align=center>

![107-6](https://cdn.codegoudan.com/img/107-6.png)

</div>

```Python
# 存储当前层的孩子节点列表
childNodes = []

for node in queue:
    # 若节点存在左孩子，入队
    if node.left:
        childNodes.append(node.left)
    # 若节点存在右孩子，入队
    if node.right:
        childNodes.append(node.right)
# 更新队列为下一层的节点，继续遍历
queue = childNodes
```



当然还是要记得对最后的结果进行逆序输出。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        if root == None:
            return []

        res = []
        queue = [root]

        while queue:
            res.append([node.val for node in queue])
            # 存储当前层的孩子节点列表
            childNodes = []

            for node in queue:
                # 若节点存在左孩子，入队
                if node.left:
                    childNodes.append(node.left)
                # 若节点存在右孩子，入队
                if node.right:
                    childNodes.append(node.right)
            # 更新队列为下一层的节点，继续遍历
            queue = childNodes

        return res[::-1]
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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> list = new ArrayList<List<Integer>>();
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (root == null) {
            return res;
        }

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            List<Integer> curlevel = new ArrayList<Integer>();
            int curlevelLen = queue.size();
            for (int i = 1; i <= curlevelLen; ++i) {
                TreeNode node = queue.poll();
                curlevel.add(node.val);

                if (node.left != null) {
                    queue.offer(node.left);
                }

                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            list.add(curlevel);
        }
        
        for(int i = list.size()-1; i >= 0; i--){
            res.add(list.get(i));
        }

        return res;
    }
}
```



二叉树的层次遍历非递归版，每个节点进出队列各一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列和一个结果数组，所以**空间复杂度为 O(n)**。



---

**图解二叉树的层序遍历Ⅱ** 到这就结束辣，大家学废了嘛？

这道题掌握了层序遍历就掌握了精髓，还是要多多练习呀！

当然不要忘记我的 **点赞，**让我快乐快乐。

我是帅蛋，我们下次见！
