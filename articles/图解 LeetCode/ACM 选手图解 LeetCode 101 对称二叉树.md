大家好呀，我是对称蛋。

今天解决**对称二叉树**，比较有意思的经典题目，检查二叉树是否是镜像对称。

不比比，直接开搞。

<div align=center>

![101-0](https://cdn.codegoudan.com/img/101-0.png)

</div>



#  LeetCode 101：对称二叉树

题目链接：[对称二叉树](https://leetcode.cn/problems/symmetric-tree/)



## 题意

给定一个二叉树，检查它是否是镜像对称的。



## 示例

二叉树 [1,2,2,3,4,4,3] 镜像对称。

<div align=center>

![101-1](https://cdn.codegoudan.com/img/101-1.png)

</div>

二叉树 [1,2,3,null,3,null,3] 非镜像对称。

<div align=center>

![101-2](https://cdn.codegoudan.com/img/101-2.png)

</div>



## 提示

- 爱帅蛋
- 么么哒



# 题目解析

题目经典，难度简单。

这道题又是说对称，又是说镜像的，看着挺唬人，其实**就是看左右子树是否相互翻转。**

<div align=center>

![101-3](https://cdn.codegoudan.com/img/101-3.png)

</div>

再直白点就是**比较两棵树**（在这道题里，根节点没什么用，把根节点拿掉来看，就是两棵树）

<div align=center>

![101-4](https://cdn.codegoudan.com/img/101-4.png)

</div>

那怎么比较呢？

我来把图的颜色稍作修改，来看下图：

<div align=center>

![101-5](https://cdn.codegoudan.com/img/101-5.png)

</div>

是不是就很一目了然？

**左子树的左节点和右子树的右节点，左子树的右节点和右子树的左节点比较即可。**

如果一棵树的左子树的左子树和右子树的右子树相等，左子树的右子树和右子树的左子树相等，那这棵树就是对称的。

<div align=center>

![101-6](https://cdn.codegoudan.com/img/101-6.jpg)

</div>



# 递归法

还是老套路，根据【[**递归算法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)】文章中讲的，实现递归，需要两步：

- 找出重复的子问题（递推公式）。
- 终止条件。

**(1) 找出重复的子问题。**

这个在上面看图的时候其实已经找出来了。

对于每一层来说，**比较的都是左子树的左节点和右子树的右节点，左子树的右节点和右子树的左节点是否相等**。

如果相等就返回 true，不相等就返回 false。

```Python
# 使用递归，对接下来的每一层做判断
# 判断左子树的左子树和右子树的右子树是否相等
leftrightTree = self.compareTree(left.left, right.right)
# 判断左子树的右子树和右子树的左子树是否相等
rightleftTree = self.compareTree(left.right, right.left)

isCompareTree = leftrightTree and rightleftTree  
```



**(2) 确定终止条件。**

对称二叉树这道题因为比较的是节点的值是否相同，所以涉及的情况有点多。

首先是**节点为空的情况**，这种必须首先考虑，不然会出现空指针的比较。

节点为空，分为 3 种情况：

- 左右节点都为空，此时相当于只有个头节点，是对称的。
- 左节点为空，右节点不为空，显然不对称。
- 左节点不为空，右节点为空，显然也是不对称的。

节点为空的情况判断完了，剩下就是**节点不为空的情况**，节点不为空，其实终止就 1 种情况：

- 比较两个节点的值，如果不相等，则不对称。

```Python
# 若左右节点不存在（即只有根节点）
if left == None and right == None:
    return True
# 左节点为空右节点不为空或左节点不为空右节点为空，则不对称
elif (left == None and right != None) or (left != None and right == None):
    return False
# 左右节点不为空，但数值不等，则不对称
elif left.val != right.val:
    return False
```



递归二步曲确定完了，这道题其实也就做出来了。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:

    def compareTree(self, left, right):
        # 若左右节点不存在（即只有根节点）
        if left == None and right == None:
            return True
        # 左节点为空右节点不为空或左节点不为空右节点为空，则不对称
        elif (left == None and right != None) or (left != None and right == None):
            return False
        # 左右节点不为空，但数值不等，则不对称
        elif left.val != right.val:
            return False

        # 使用递归，对接下来的每一层做判断
        # 判断左子树的左子树和右子树的右子树是否相等
        leftrightTree = self.compareTree(left.left, right.right)
        # 判断左子树的右子树和右子树的左子树是否相等
        rightleftTree = self.compareTree(left.right, right.left)
        
        isCompareTree = leftrightTree and rightleftTree 
        
        return isCompareTree

    def isSymmetric(self, root: TreeNode) -> bool:
        # 空树
        if root == None:
            return True
        
        return self.compareTree(root.left, root.right)
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
  public boolean isSymmetric(TreeNode root) {
    if(root == null) {
      return true;
    }
    
    return compareTree(root.left,root.right);
  }
  
  boolean compareTree(TreeNode left, TreeNode right) {
        // 若左右节点不存在（即只有根节点）
    if(left == null && right == null) {
      return true;
    }
        // 左节点为空右节点不为空或左节点不为空右节点为空，则不对称
    if((left == null && right != null) || (left != null && right == null)) {
      return false;
    }
        // 左右节点不为空，但数值不等，则不对称
    if(left.val!=right.val) {
      return false;
    }
    
        // 判断左子树的左子树和右子树的右子树是否相等
        boolean leftrightTree = compareTree(left.left,right.right);
        // 判断左子树的右子树和右子树的左子树是否相等
        boolean rightleftTree = compareTree(left.right,right.left);
    
        boolean isCompareTree = leftrightTree && rightleftTree;

        return isCompareTree;
  }
}
```

此解法，由于每个节点被遍历一次，所以**时间复杂度为 O(n)**。

使用递归，在过程中额外调用了栈空间，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

在递归法法中我说过：对于每一层来说，比较的都是左子树的左节点和右子树的右节点，左子树的右节点和右子树的左节点是否相等。

其实这有点类似层次遍历。

**每次将当前层的节点，按照左子树的左节点、右子树的右节点、左子树的右节点和右子树的左节点放入队列，再依次两两出队列，比较数值是否相等。**

比如对于下图：

<div align=center>

![101-6](https://cdn.codegoudan.com/img/101-6.png)

</div>

首先初始化队列，并将根节点的左右节点入队列。

<div align=center>

![101-7](https://cdn.codegoudan.com/img/101-7.png)

</div>

```Python
# 初始化队列
queue = [root.left, root.right]
```



当队列不为空，将前两个元素出队列进行比较。

<div align=center>

![101-8](https://cdn.codegoudan.com/img/101-8.png)

</div>

```Python
# 从队列中取出两个节点
left = queue.pop(0)
right = queue.pop(0)
# 若两个节点都为空（左右节点不存在），则继续循环
if left == None and right == None:
    continue
# 左节点为空右节点不为空或左节点不为空右节点为空，则不对称
elif (left == None and right != None) or (left != None and right == None):
    return False
# 左右节点不为空，但数值不等，则不对称
elif left.val != right.val:
    return False
```



下面再依次将左子树的左节点、右子树的右节点、左子树的右节点和右子树的左节点放入队列。

<div align=center>

![101-9](https://cdn.codegoudan.com/img/101-9.png)

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
    def isSymmetric(self, root: TreeNode) -> bool:
        if root == None:
            return True

        # 初始化队列
        queue = [root.left, root.right]
        while queue:
            # 从队列中取出两个节点
            left = queue.pop(0)
            right = queue.pop(0)
            # 若两个节点都为空（左右节点不存在），则继续循环
            if left == None and right == None:
                continue
            # 左节点为空右节点不为空或左节点不为空右节点为空，则不对称
            elif (left == None and right != None) or (left != None and right == None):
                return False
            # 左右节点不为空，但数值不等，则不对称
            elif left.val != right.val:
                return False

            # 左节点的左孩子和右节点的右孩子入队列
            queue.append(left.left)
            queue.append(right.right)
            # 左节点的右孩子和右节点的左孩子入队列
            queue.append(left.right)
            queue.append(right.left)
        
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
  public boolean isSymmetric(TreeNode root) {
    if(root==null) {
      return true;
    }
    // 初始化队列
    LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root.left);
    queue.add(root.right);
    while(queue.size() > 0) {
      // 从队列中取出两个节点
      TreeNode left = queue.removeFirst();
      TreeNode right = queue.removeFirst();
      // 若两个节点都为空（左右节点不存在），则继续循环
      if(left == null && right == null) {
        continue;
      }
            // 左节点为空右节点不为空或左节点不为空右节点为空，则不对称
      if((left == null && right != null) || (left != null && right == null)){
        return false;
      }
            // 左右节点不为空，但数值不等，则不对称
      if(left.val != right.val) {
        return false;
      }
      // 左节点的左孩子和右节点的右孩子入队列
      queue.add(left.left);
      queue.add(right.right);
      // 左节点的右孩子和右节点的左孩子入队列
      queue.add(left.right);
      queue.add(right.left);
    }
    return true;
  }
}
```



同样，非递归法，**时间复杂度为 O(n)，空间复杂度为 O(n)**。



---

**图解对称二叉树**到这就结束辣，大噶伙儿学废了嘛？

是不是感觉还挺简单的，我一直觉得，二叉树的题目还蛮有意思的。

一道题，拆着拆着就拆成了自己认识的样子。

二叉树这个专题，我会带大家多做一些实战题，争取学完就搞懂。

大家加油，当然也要记得给本蛋**点赞 **，给我加加油！

我是帅蛋，我们下次见辣！ 
