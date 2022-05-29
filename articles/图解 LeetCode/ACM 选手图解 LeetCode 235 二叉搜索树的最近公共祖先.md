大家好呀，我是帅蛋。

今天解决**二叉搜索树的最近公共祖先**，之前我们一起做过【[**二叉树的最近公共祖先**](https://mp.weixin.qq.com/s/MmlenpO4wUrgH7FcDgptiA)】这道题，如果你认真看过，那这道题对你来说么的问题。

什么？你说你没看过？问题不大，看这篇也一样~

<div align=center>

![235-5](https://cdn.codegoudan.com/img/235-5.jpg)

</div>

那...小板凳搬好，我们赶紧开始！

<div align=center>

![235-0](https://cdn.codegoudan.com/img/235-0.png)

</div>



# LeetCode 235：二叉搜索树的最近公共祖先

题目链接：[二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)



## 题意

给定一个二叉搜索树，找到该树中两个指定节点的最近公共祖先。



## 示例

输入：root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4

输出：2

解释：节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。

<div align=center>

![235-1](https://cdn.codegoudan.com/img/235-1.png)

</div>



## 提示

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉搜索树中。



# 题目解析

二叉搜索树的最近公关祖先这道题，难度简单。因为自带的性质，比之前[**二叉树的最近公共祖先**](https://mp.weixin.qq.com/s/MmlenpO4wUrgH7FcDgptiA)难度直接低了一个 level。

<div align=center>

![235-6](https://cdn.codegoudan.com/img/235-6.jpg)

</div>

题目中对于公共祖先的定义是这样的：对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大。

你看，说的真好，听君一席话，如听一席话！

<div align=center>

![235-7](https://cdn.codegoudan.com/img/235-7.jpg)

</div>

我在二叉树的最近公共祖先中说过，**对于最近公共祖先**，我感觉你们就记住，**对于节点 p 和 q 来说，如果 node 为其最近公共祖先，那么 node 的左孩子和右孩子一定不是 p 和 q 的公共祖先**。

比如对于下图， 如果 p = 7、q = 0，3 就是 p 和 q 的最近公共祖先（3 的左孩子 5 和右孩子 1 都不是 p 和 q 的公共祖先）。

<div align=center>

![235-8](https://cdn.codegoudan.com/img/235-8.png)

</div>

也正是根据这个，我们得出**对于普通的二叉树**，如果节点 node 为 p 和 q 的最近公共祖先，那么会有 **3** 种情况：

- p 和 q 分别在节点 node 的左右子树中。
- node 即为节点 p，q 在节点 p 的左子树或右子树中。
- node 即为节点 q，p 在节点 q 的左子树或者右子树中。本来按照上面的情况枚举 5 种情况就完事了，但是咱二叉搜索树带性质啊：**二叉搜索树是一棵有序树！**

<div align=center>

![235-9](https://cdn.codegoudan.com/img/235-9.jpg)

</div>

就这一个【有序】就赢了！简单了好多：

- 如果当前节点的值 node.val 在 [p, q] 之间，那 node 就是最近公共祖先。
- 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中，向左子树继续遍历。
- 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中，向右子树继续遍历。

<div align=center>

![235-10](https://cdn.codegoudan.com/img/235-10.jpg)

</div>



# 递归法

用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是老办法，掏出递归二步曲：



**(1) 找出重复的子问题。**

这里的重复子问题就很简单，就是递归左子树，递归右子树：

- 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中，向左子树继续遍历。
- 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中，向右子树继续遍历。

```Python
# 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中
if root.val > p.val and root.val > q.val:
    # 遍历左子树
    return self.lowestCommonAncestor(root.left, p, q)
# 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中
if root.val < p.val and root.val < q.val:
    # 遍历右子树
    return self.lowestCommonAncestor(root.right, p, q)
```



**(2) 确定终止条件。**

对于本题来说，终止条件就一个，那就是找到了就结束：

```Python
return root
```



这两点确定好了，最后的代码也就出来了。

<div align=center>

![235-11](https://cdn.codegoudan.com/img/235-11.jpg)

</div>



## Python 代码实现

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中
        if root.val > p.val and root.val > q.val:
            # 遍历左子树
            return self.lowestCommonAncestor(root.left, p, q)
        # 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中
        if root.val < p.val and root.val < q.val:
            # 遍历右子树
            return self.lowestCommonAncestor(root.right, p, q)
        # 如果当前节点的值 node.val 在 [p, q] 之间，那 node 就是最近公共祖先。
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
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中
        if(root.val > p.val && root.val > q.val){
            // 遍历左子树
            return lowestCommonAncestor(root.left, p, q);
        }
        // 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中
        if(root.val < p.val && root.val < q.val){
            // 遍历右子树
            return lowestCommonAncestor(root.right, p, q);
        }
        // 如果当前节点的值 node.val 在 [p, q] 之间，那 node 就是最近公共祖先。
        return root;
    }
}
```



此解法，最坏情况下每个节点被遍历一次，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉搜索树的高度，二叉搜索树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

非递归法的分析思路和递归法的分析思路一样，也还是那 3 种情况：

- 如果当前节点的值 node.val 在 [p, q] 之间，那 node 就是最近公共祖先。
- 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中，向左子树继续遍历。
- 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中，向右子树继续遍历。

以下图为例：

<div align=center>

![235-2](https://cdn.codegoudan.com/img/235-2.png)

</div>

当 p = 2，q = 4 时，我对上图的 p、q 节点做了标记。

<div align=center>

![235-3](https://cdn.codegoudan.com/img/235-3.png)

</div>

第 1 步，root 的值为 6，此时 root.val > p.val 和 > q.val，证明 p 和 q 都在 root 的左子树上，继续遍历左子树。

<div align=center>

![235-4](https://cdn.codegoudan.com/img/235-4.png)

</div>

```Python
# 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中
if root.val > p.val and root.val > q.val:
    # 继续向左
    root = root.left
```



第 2 步，root 的值为 2，此时 root.val ≥ p.val 且 root.val < q.val，所以此二叉搜索树的最近公共祖先直接就是 root.val = 2。

```Python
# 如果当前节点的值 node.val 在 [p, q] 之间，那 node 就是最近公共祖先。
else:
    # 找到了
    return root
```



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while True:
            # 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中
            if root.val > p.val and root.val > q.val:
                # 继续向左
                root = root.left
            # 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中
            elif root.val < p.val and root.val < q.val:
                # 继续向右
                root = root.right
            # 如果当前节点的值 node.val 在 [p, q] 之间，那 node 就是最近公共祖先。
            else:
                # 找到了
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
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while(true){
            // 如果当前节点的值 node.val 大于 p 和 q 的值，那证明 p 和 q 在 node 的左子树中
            if(root.val > p.val && root.val > q.val){
                // 继续向左
                root = root.left;
            }
            // 如果当前节点的值 node.val 小于 p 和 q 的值，那证明 p 和 q 在 node 的右子树中
            else if(root.val < p.val && root.val < q.val){
                // 继续向右
                root = root.right;
            }
            // 如果当前节点的值 node.val 在 [p, q] 之间，那 node 就是最近公共祖先
            else{
                // 找到了
                return root;
            }
        }
    }
}
```



同样此解法**时间复杂度为 O(n)**，但是由于没有使用额外的空间，此时的**空间复杂度为 O(1)**。



---

**图解二叉搜索树的最近公共祖先**到这就结束辣，二叉搜索树自身的性质真的会给做题带来太多的方便啦，爱了爱了！

<div align=center>

![235-12](https://cdn.codegoudan.com/img/235-12.jpg)

</div>

我是帅蛋，我们下次继续搞起~
