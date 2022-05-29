大家好呀，我是 N 叉烧蛋。

<div align=center>

![429-0](https://cdn.codegoudan.com/img/429-0.jpg)

</div>

之前的文章中我讲了二叉树的层次遍历，说了递归和非递归两种方法：



[**ACM 选手带你玩转二叉树层次遍历（递归 + 非递归）**](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)



**层次遍历就是表面意思，一层层的遍历，同一层的遍历按照从左到右逐个遍历。**

今天来解决 **N 叉树的层序遍历**，不一样的叉，一样的套路，检查你之前学的是不是已经掌握了。

<div align=center>

![429-1](https://cdn.codegoudan.com/img/429-1.jpg)

</div>

那下面我们就来搞搞这道题。

<div align=center>

![429-2](https://cdn.codegoudan.com/img/429-2.png)

</div>



# LeetCode 429：N 叉树的层序遍历

题目链接：[N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)



## 题意

给定一个 N 叉树，返回其节点值的层序遍历。

树的序列化输入是用层序遍历，没组子节点都由 null 值分隔。



## 示例

输入：root = [1,null,3,2,4,null,5,6]

输出：[[1],[3,2,4],[5,6]]



## 提示

- 树的高度不会超过 1000
- 树的节点总数在 [0,10^4] 之间



# 递归版



## 解析

我在【[**二叉树层次遍历**](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)】中说过，层次遍历使用递归法，有点反层次遍历的初衷。

层次遍历就是要一层一层的遍历，而我们都知道递归是一种解决到底然后再去解决其它问题的特点，也就是在深度。

使用递归法，就需要祭出递归的二步曲：

**(1) 找出重复的子问题。**

层次遍历是每一层的节点从左到右的遍历，所以**在遍历的时候我们可以先遍历最左边的子树，再遍历靠左的子树，...，一直遍历完最右边的子树**。

<div align=center>

![429-3](https://cdn.codegoudan.com/img/429-3.png)

</div>

遍历的顺序是：1 -> 3 -> 5 -> 3 -> 6 -> 3 -> 1 -> 2 -> 1 -> 4。

```Python
# 递归处理子树
for child in root.children:
    self.level(child, depth+1, res)
```

需要注意的是，在遍历子树的时候，涉及到向上或者向下遍历，为了让递归的过程中的同一层的节点放在同一个列表中，**在递归时要记录深度 depth**。

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

最重要的两步确定完了，那代码也就出来了。

二叉树层次遍历递归版，由于每个节点都被遍历到，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，还维护了一个 res 的结果数组，所以**空间复杂度为 O(n)**。



## 代码实现



### Python 代码实现

```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:

    def level(self,root: 'Node', depth, res):
        if root == None:
            return []
        # 若当前行对应的列表不存在，加一个空列表
        if len(res) < depth:
            res.append([])
        # 将当前节点的值加入当前行的 res 中
        res[depth - 1].append(root.val)
        # 递归处理子树
        for child in root.children:
            self.level(child, depth+1, res)

    def levelOrder(self, root: 'Node') -> List[List[int]]:

        res = []
        self.level(root, 1, res)
        
        return res
```



### Java 代码实现

```Java
class Solution {

    private List<List<Integer>> res = new ArrayList<>();

    private void level(Node root, int depth) {
        if (res.size() < depth) {
            res.add(new ArrayList<>());
        }
        res.get(depth - 1).add(root.val);
        for (Node child : root.children) {
            level(child, depth + 1);
        }
    }

    public List<List<Integer>> levelOrder(Node root) {
        if (root != null){
            level(root, 1);
        }
        return res;
    }
}
```



# 非递归版



## 解析

**由于层次遍历的属性非常契合队列的特点，所以非递归版的层次遍历用的是队列。**

队列是一种**先进先出**（First in First Out）的数据结构，简称 **FIFO**。

如果不太了解队列的，可以看下面这篇文章：



**[ACM 选手带你玩转栈和队列](https://mp.weixin.qq.com/s/4D0FQiJMJ9wA4_ejkGbwiA)**



思路很简单：

**使用队列保存每一层的所有节点，把队列里的所有节点出队列，然后把这些出去节点各自的子节点入队列**。

以此来完成对每层的遍历。

<div align=center>

![429-4](https://cdn.codegoudan.com/img/429-4.png)

</div>

N 叉树的层次遍历非递归版，每个节点进出队列各一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列和一个结果数组，所以**空间复杂度为 O(n)**。



## 代码实现



### Python 代码实现

```Python
class Solution:

    def levelOrder(self, root: 'Node') -> List[List[int]]:

        if root == None:
            return []

        res = []
        queue = [root]

        while queue:
            res.append([node.val for node in queue])
            # 存储当前层的孩子节点列表
            childNodes = []
            for node in queue:
                # 若节点存在子节点，入队
                if node.children:
                    childNodes.extend(node.children)
            # 更新队列为下一层的节点，继续遍历
            queue = childNodes

        return res
```



### Java 代码实现

```Java
class Solution {
        public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if (root == null) {
            return res;
        }

        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            List<Integer> curlevel = new ArrayList<Integer>();
            int curlevelLen = queue.size();
            for (int i = 1; i <= curlevelLen; ++i) {
                Node node = queue.poll();
                curlevel.add(node.val);
                queue.addAll(node.children);
            }
            res.add(curlevel);
        }
        return res;
    }
}
```



---

**图解 N 叉树的层序遍历**到这就结束辣，你看我说的对吧，不管多数叉，不同的叉，一样的套路。

虽然 LeetCode 定义为难度中等的题，但是**搞懂了原理，就和切水题一样简单**。

还是那句话，**只要大家认真去学，认真的思考，就一定能快乐的切题**。

当然，也要记得**点赞 **，让我也快乐呀~

我是帅蛋，我们下次见！
