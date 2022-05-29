今天我要来 cos 一下“园丁”，来修剪修剪这棵二叉搜索树，这可是个体力活。

<div align=center>

![669-0](https://cdn.codegoudan.com/img/669-0.jpg)

</div>

话不多说，掏出小剪刀，我们修起来~

<div align=center>

![669-1](https://cdn.codegoudan.com/img/669-1.png)

</div>



#  LeetCode 669：修剪二叉搜索树

题目链接：[修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)



## 题意

给定二叉搜索树的根节点 root，以及最小边界 low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在 [low,high] 中，结果返回修剪好的二叉搜索树的新的根节点。

注意：修剪树不应该改变保留在树中的元素的相对结构（即，如果没有被移除，原有的父代子代关系都应当保留）。



## 示例

输入：root = [3,0,4,null,2,null,null,1]，low = 1，high = 3

输出：[3,2,null,1]

<div align=center>

![669-2](https://cdn.codegoudan.com/img/669-2.png)

</div>



## 提示

- 树中节点数在范围 [1, 10^4] 内
- 0 <= Node.val <= 10^4
- 树中的每个节点的值都是唯一的
- 题目数据保证输入是一棵有效的二叉搜索树
- 0 <= low <= high <= 10^4



# 题目解析

修剪二叉搜索树，难度中等。

整个题目描述看起来很长，其实意思就是将二叉搜索树在 [low, high] 之外的节点值删掉。

如果你看过我之前【[**删除二叉搜索树中的节点**](https://mp.weixin.qq.com/s/k3MCptwFIhiRNdsXaegsRA)】这篇文章，你会发现，其实类似批量删除二叉搜索树中的节点。

仅仅是删除不难，还是我之前说过的，难就难在删除节点之后仍然要保持剩下的二叉树依然是二叉搜索树。

<div align=center>

![669-3](https://cdn.codegoudan.com/img/669-3.jpg)

</div>



# 递归法

根据【[**递归算法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)】文章中讲的，实现递归，需要两步：

- 找出重复的子问题（递推公式）。
- 终止条件。



**(1) 找出重复的子问题。**

给定了区间 [low, high]，修剪完之后的二叉搜索树的每个节点值都要在这个区间内。

那对于原始二叉搜索树的每个节点来说，存在 3 种情况：

- 当前节点值 < 左边界 low。

这种情况下了，根据二叉搜索树的特性，只需要考虑当前节点的右子树，左子树肯定是都去掉的。

```Python
# 如果当前节点值 < 左边界，根据二叉搜索树的特性，只需要考虑右子树，抛弃左子树
if root.val < low:
    return self.trimBST(root.right, low, high)
```



- 当前节点值 > 右边界 high。

这种情况下，根据二叉搜索树的特性，只需要考虑当前节点的左子树，右子树则肯定都是去掉的。

```Python
# 如果当前节点值 > 右边界，根据二叉搜索树特性，只需要考虑左子树，抛弃右子树
if root.val > high:
    return self.trimBST(root.left, low, high)
```



- 当前节点值在 [low, high] 之间。

在这个时候，根节点不需要修剪，那就是继续递归修剪左子树和右子树，左子树的结果返回给 root.left，右子树的结果返回给 root.right。

```Python
# root 在 [low, high] 区间内
if low <= root.val <= high:
   root.left = self.trimBST(root.left, low, high)
   root.right = self.trimBST(root.right, low, high)
   return root
```



**(2) 确定终止条件。**

对于终止条件就没啥好说的，遍历到空节点，直接返回就 ok 了。

```Python
if root == None:
    return None
```



这两点确定好了，下面我们直接来看代码。

<div align=center>

![669-4](https://cdn.codegoudan.com/img/669-4.jpg)

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
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if root == None:
            return None
        # 如果当前节点值 < 左边界，根据二叉搜索树的特性，只需要考虑右子树，抛弃左子树
        if root.val < low:
            return self.trimBST(root.right, low, high)
        # 如果当前节点值 > 右边界，根据二叉搜索树特性，只需要考虑左子树，抛弃右子树
        if root.val > high:
            return self.trimBST(root.left, low, high)
        # root 在 [low, high] 区间内
        if low <= root.val <= high:
            root.left = self.trimBST(root.left, low, high)
            root.right = self.trimBST(root.right, low, high)
            return root
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
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if(root == null){
            return null;
        }
        // 如果当前节点值 < 左边界，根据二叉搜索树的特性，只需要考虑右子树，抛弃左子树
        if(root.val < low){
            return trimBST(root.right, low, high);
        }
        // 如果当前节点值 > 右边界，根据二叉搜索树特性，只需要考虑左子树，抛弃右子树
        if(root.val > high){
            return trimBST(root.left, low, high);
        }
        // root 在 [low, high] 区间内
        if(root.val >= low && root.val <= high){
            root.left = trimBST(root.left, low, high);
            root.right = trimBST(root.right, low, high);
        }
        return  root;
    }
}
```



本题解，每个节点都要访问一次，所以**时间复杂度为 O(n)**。

在递归过程中调用了额外的栈空间，栈的大小取决于二叉搜索树的高度，同样二叉搜索树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



## 非递归法（迭代）

一般我们在用非递归法的时候，都是用栈来模拟递归，但是因为**二叉搜索树是有序**的特性，其实直接按照下面的方式直接模拟即可：

- 找到第 1 个在 [low, high] 之间的节点（这个节点就是新的二叉搜索树的根节点），对这个节点的左右子树做修剪。
- 先遍历左子树，对于左子树的节点，如果当前节点 cur 的左子树存在 且 cur.left.val < 左边界 low，直接把 cur.left 指向 cur.left.right 节点。
- 再遍历右子树，对于右子树的节点，如果当前节点 cur 的右子树存在且 cur.right.val > 右边界 high，直接把 cur.right 指向 cur.right.left 节点。

上面 3 步一定要仔细揣摩，草稿纸走起来！

<div align=center>

![669-5](https://cdn.codegoudan.com/img/669-5.png)

</div>

我们以下图为例：

<div align=center>

![669-6](https://cdn.codegoudan.com/img/669-6.png)

</div>

对于它的修剪其实就是如下图：

<div align=center>

![669-7](https://cdn.codegoudan.com/img/669-7.png)

</div>

去掉过程，我们最终得到的就是下图的二叉搜索树：

<div align=center>

![669-8](https://cdn.codegoudan.com/img/669-8.png)

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
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if(root == None):
            return None
        # 找到修剪之后的二叉搜索树的头节点 root
        while root and (root.val > high or root.val < low):
            if root.val > high:
                root = root.left
            else:
                root = root.right
        # 修剪 root 的左子树，将 < low 的节点删除
        cur = root
        while cur:
            while cur.left and cur.left.val < low:
                cur.left = cur.left.right
            cur = cur.left
        # 修剪 root 的右子树，将 > high 的节点删除
        cur = root
        while cur:
            while cur.right and cur.right.val > high:
                cur.right = cur.right.left
            cur = cur.right
        
        return root
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
    public TreeNode trimBST(TreeNode root, int low, int high) {
        // 找到修剪之后的二叉搜索树的头节点 root
        if(root == null){
            return null;
        }
        while(root != null && (root.val > high || root.val < low)){
            if(root.val > high){
                root = root.left;
            }
            else{
                root = root.right;
            }
        }
        TreeNode cur = root;
        // 修剪 root 的左子树，将 < low 的节点删除
        while(cur != null){
            while(cur.left != null && cur.left.val < low){
                cur.left = cur.left.right;
            }
            cur = cur.left;
        }
        // 修剪 root 的右子树，将 > high 的节点删除
        cur = root;
        while(cur != null){
            while(cur.right != null && cur.right.val > high){
                cur.right = cur.right.left;
            }
            cur = cur.right;
        }
        return root;
    }
}
```



同样，非递归法的**时间复杂度为 O(n)**，对于空间复杂度，非递归法只额外开辟了常数级的空间，所以**空间复杂度为 O(1)**。



---

**图解修剪二叉搜索树**到这就结束辣，大家学废了嘛？

相信二叉搜索树的实战题目做到现在，大家已经有点感觉了，感谢二叉搜索树的性质，其实很多时候在做题的时候我感觉比二叉树简单多了。



