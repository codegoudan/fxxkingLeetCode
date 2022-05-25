大家好呀，我是在第 18 层的帅蛋。

**我在之前说过，二叉树遍历的实现历来是面试的高频问题。**

这里提到的二叉树遍历一般就是：前序遍历、中序遍历、后序遍历和层次遍历。

关于二叉树的前中后序遍历有两种实现方式：**递归和非递归**。我在前面两篇文章已经做了介绍：



[**ACM 选手带你玩转二叉树前中后序遍历（递归版）**](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)

[**ACM 选手带你玩转二叉树前中后序遍历（非递归版）**](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



没看过的小婊贝可以看一下。

今天呢，就来讲剩下的层次遍历。

**层次遍历就是表面意思，一层层的遍历，同一层的遍历按照从左到右逐个遍历。**

<div align=center>

![ccbl-0](https://cdn.codegoudan.com/img/ccbl-0.png)

</div>

像上面这个二叉树，它的层次遍历顺序为：ABCDEFGHIJ。

关于二叉树层次遍历，同样也是有递归和非递归两种实现方式，但是**层次遍历因为它的遍历属性，和之前的前中后序遍历还是有些不同的**。

<div align=center>

![ccbl-1](https://cdn.codegoudan.com/img/ccbl-1.png)

</div>

下面我用一道 LeetCode 的题来给大家具体讲解二叉树层次遍历的递归和非递归方法。



# 二叉树的层次遍历



## LeetCode 102：二叉树的层次遍历

给你一个二叉树，请你返回其按层次遍历得到的节点值。



## 示例

<div align=center>

![ccbl-2](https://cdn.codegoudan.com/img/ccbl-2.png)

</div>



# 递归版



## 解析

二叉树层次遍历使用递归法，说实话有点反层次遍历的初衷。

人家层次遍历就是要一层一层的遍历，而我们都知道递归是一种解决到底然后再去解决其它问题的特点，也就是深度，就像下面这样：

<div align=center>

![ccbl-3](https://cdn.codegoudan.com/img/ccbl-3.png)

</div>

既然准备使用递归，那就寄出递归二步曲：

**(1) 找出重复的子问题。**

层次遍历是每一层的节点从左到右的遍历，所以**在遍历的时候我们可以先遍历左子树，再遍历右子树**。

<div align=center>

![ccbl-4](https://cdn.codegoudan.com/img/ccbl-4.png)

</div>

可能换成下面这样更清楚点：

<div align=center>

![ccbl-5](https://cdn.codegoudan.com/img/ccbl-5.png)

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

还是那句话，最重要的两点确定完了，那代码也就出来了。

二叉树层次遍历递归版，由于每个节点都被遍历到，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，维护了一个 res 的结果数组，所以**空间复杂度为 O(n)**。



## 代码实现



### Python 代码实现

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


    def levelOrder(self, root: TreeNode) -> List[List[int]]:

        res = []
        self.level(root, 1, res)

        return res
```



### Java 代码实现

```Java
class Solution {

    void level(TreeNode root, int index, List<List<Integer>> res) {
    if(res.size() < index) {
      res.add(new ArrayList<Integer>());
    }

    res.get(index-1).add(root.val);

    if(root.left != null) {
      level(root.left, index+1, res);
    }

    if(root.right != null) {
      level(root.right, index+1, res);
    }
  }

  public List<List<Integer>> levelOrder(TreeNode root) {
    if(root == null) {
      return new ArrayList<List<Integer>>();
    }

    List<List<Integer>> res = new ArrayList<List<Integer>>();
    level(root, 1, res);
    return res;
  }
}
```



# 非递归版（迭代）



## 解析

**非递归版的层次遍历和之前讲的非递归版的前中后序遍历，还是不同的**。

非递归版的前中后续遍历，是将递归中用的栈光明正大的模拟出来。

而**非递归版的层次遍历我们用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

队列是一种**先进先出**（First in First Out）的数据结构，简称 **FIFO**。

如果不太了解队列的，可以看下面这篇文章：



[ACM 选手带你玩转栈和队列](https://mp.weixin.qq.com/s/4D0FQiJMJ9wA4_ejkGbwiA)



思路很简单：

**使用队列保存每一层的所有节点，把队列里的所有节点出队列，然后把这些出去节点各自的子节点入队列**。

以此来完成对每层的遍历。

<div align=center>

![ccbl-6](https://cdn.codegoudan.com/img/ccbl-6.png)

</div>

```python
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

二叉树的层次遍历非递归版，每个节点进出队列各一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列和一个结果数组，所以**空间复杂度为 O(n)**。



## 代码实现



### Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:

        if　root == None:
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
        return res
```



### Java 代码实现

```Java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
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
            res.add(curlevel);
        }

        return res;
    }
}
```



---



到这，**二叉树的层次遍历（递归 + 非递归）**就讲完啦，大家学废了嘛？

至此，二叉树的四种遍历我都已经完整的介绍了，希望大家能够认真的看完，认真的给我点赞呀。

我是帅蛋，我们下次见~
