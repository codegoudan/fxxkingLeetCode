大家好呀，我是在爬二叉树的帅蛋。

**二叉树遍历的实现历来是面试的高频问题**，关于二叉树的前中后序遍历有两种实现方式：**递归和非递归**。

递归实现起来简单一些，而非递归的实现需要借助之前讲过的【[**栈**](https://mp.weixin.qq.com/s/4D0FQiJMJ9wA4_ejkGbwiA)】这个数据结构

这两种实现方式都需要蛋粉们牢牢掌握，我们先从简单的开始，先讲递归版的二叉树前中后序遍历。

<div align=center>

![qzhdg-0](https://cdn.codegoudan.com/img/qzhdg-0.jpg)

</div>

关于递归，我之前写过一篇很详细的文章：



[**ACM 选手带你玩转递归算法！**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)



没看过的蛋粉可以看一下。

说白了**递归就是一种循环，一种自己调用自己的循环**。

一道题你看**能不能用递归实现，需要具备两个条件**：

- 问题可以分为多个重复的子问题，子问题仅存在数据规模的差距。
- 存在终止条件。

所以根据条件，对于**实现递归，只需要两步**：

- 找出重复的子问题（递推公式）。
- 终止条件。

话不多说，赶紧开整。

<div align=center>

![qzhdg-1](D:\书籍资料\公众号图片\前中后序遍历递归\qzhdg-1.png)

</div>

下面我用**三道 LeetCode 题**来给大家具体讲解二叉树前中后序遍历的递归版。

<div align=center>

![qzhdg-2](https://cdn.codegoudan.com/img/qzhdg-2.jpg)

</div>

# 二叉树前序遍历（递归版）



## LeetCode 144：二叉树的前序遍历

给你二叉树根节点 root，返回它节点值的前序遍历。



## 示例

<div align=center>

![qzhdg-3](https://cdn.codegoudan.com/img/qzhdg-3.png)

</div>

输入：root = [1,null,2,3]

输出：[1,2,3]



## 解析

根据上面讲的实现递归的两步来找：



### (1) 找出重复的子问题

这个很好找，前序遍历的顺序是：根、左子树、右子树。

对于左子树或者右子树来说，也是同样的遍历顺序。

所以这个重复的子问题就出来了，**先取根节点，再遍历左子树，最后遍历右子树**。

```Python
print(root.val)
preOrder(root.left)
preOrder(root.right)
```



### (2) 确定终止条件

对于二叉树的遍历来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

那就是当前的节点是空的，既然是空的那就没啥好遍历。

```Python
if root == None:
    return
```

最重要的两点确定完了，那总的代码也就出来了。



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
    # 前序遍历函数
    def preOrder(self, root: TreeNode, res):
        if root == None:
            return
        res.append(root.val)
        self.preOrder(root.left, res)
        self.preOrder(root.right, res)

    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.preOrder(root, res)
        return res
```



### Java 代码实现

```Java
class Solution {

    public void preOrder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        res.add(root.val);
        preOrder(root.left, res);
        preOrder(root.right, res);
    }
    
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        preOrder(root, res);
        return res;
    }
}
```

此解法，由于每个节点被遍历一次，所以**时间复杂度为 O(n)**。

额外维护了一个 res 列表，**空间复杂度为 O(n)**。



# 二叉树中序遍历（递归版）



## LeetCode 94：二叉树的中序遍历

给你二叉树根节点 root，返回它的中序遍历。



## 示例

<div align=center>

![qzhdg-3](https://cdn.codegoudan.com/img/qzhdg-3.png)

</div>

输入：root = [1,null,2,3]

输出：[1,3,2]



## 解析

同样根据上面讲的实现递归的两步来找：



### (1) 找出重复的子问题

中序遍历的顺序是：左子树、根、右子树。

对于左子树、右子树来说，也是同样的遍历顺序。

所以这个重复的子问题就是：**先遍历左子树、再取根节点，最后遍历右子树**。

```Python
inOrder(root.left)
print(root.val)
inOrder(root.right)
```



### (2) 确定终止条件

和前序遍历相同，就是当前的节点为空，空的没啥好遍历的。

```Python
if root == None:
    return 
```

最重要的两点确定完了，那总的代码也就出来了。



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

    def inOrder(self, root:TreeNode, res):
        if root == None:
            return
        self.inOrder(root.left, res)
        res.append(root.val)
        self.inOrder(root.right, res)

    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.inOrder(root, res)
        return res
```



### Java 代码实现

```Java
class Solution {

    public void inOrder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        inOrder(root.left, res);
        res.add(root.val);
        inOrder(root.right, res);
    }
    
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        inOrder(root, res);
        return res;
    }
}
```

和前序遍历相同，中序遍历**时间复杂度为 O(n)**，**空间复杂度为 O(n)**。



# 二叉树后序遍历（递归版）



## LeetCode 145：二叉树的后序遍历

给定一个二叉树，返回它的后序遍历。



## 示例

<div align=center>

![qzhdg-3](https://cdn.codegoudan.com/img/qzhdg-3.png)

</div>

输入：root = [1,null,2,3]

输出：[3,2,1]



## 解析

还是根据上面讲的实现递归的两步来找：



### (1) 找出重复的子问题。

后序遍历的顺序是：左子树、右子树、根，对于左子树、右子树来说，也是同样的遍历顺序。

所以这个重复的子问题就是：先遍历左子树、再遍历右子树、再取根节点。

```Python
postOrder(root.left)
postOrder(root.right)
print(root.val)
```



### (2) 确定终止条件。

和前序遍历、中序遍历相同，就是当前的节点为空，空的没啥好遍历的。

```Python
if root == None:
    return 
```

最重要的两点确定完了，那总的代码也就出来了。



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

    def postOrder(self, root: TreeNode, res):
        if root == None:
            return

        self.postOrder(root.left, res)
        self.postOrder(root.right, res)
        res.append(root.val)

    def postorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        self.postOrder(root, res)
        return res
```



### Java 代码实现

```Java
class Solution {

    public void postorder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        postorder(root.left, res);
        postorder(root.right, res);
        res.add(root.val);
    }
    
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        postorder(root, res);
        return res;
    }
}

```

和前序遍历、中序遍历相同，后序遍历**时间复杂度为 O(n)**，**空间复杂度为 O(n)**。



---

好啦，到这**递归版的二叉树的前中后序遍历**就讲完啦。

是不是蛋粉们觉得很简单？

如果因此你就小看了二叉树遍历，那你就图样图森破，非递归的二叉树遍历还是不那么简单的。

当然啦，我肯定不会说，不那么简单还是简单。

<div align=center>

![qzhdg-4](https://cdn.codegoudan.com/img/qzhdg-4.jpg)

</div>

