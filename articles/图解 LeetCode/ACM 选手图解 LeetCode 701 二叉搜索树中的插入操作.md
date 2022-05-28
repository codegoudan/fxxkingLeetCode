大家好呀，我是插入蛋。

今天解决**二叉搜索树中的插入操作**，二叉搜索树 3 大操作之一，非常重要的内容，记得搬好板凳，记好笔记。

下面让我们赶紧搞起~

<div align=center>

![701-0](https://cdn.codegoudan.com/img/701-0.png)

</div>



# LeetCode 701：二叉搜索树中的插入操作



## 题意

给定二叉搜索树的根节点 root 和要插入的数值 value，将值插入二叉搜索树，返回插入后二叉搜索树的根节点。

输入数据保证，新值和原始二叉搜索树中的任意节点值都不同。



## 示例

输入：root = [4,2,7,1,3], val = 5
输出：[4,2,7,1,3,5]

<div align=center>

![701-9](https://cdn.codegoudan.com/img/701-9.png)

</div>

输出存在两种情况：

<div align=center>

![701-2](https://cdn.codegoudan.com/img/701-2.png)

![701-3](https://cdn.codegoudan.com/img/701-3.png)

</div>

## 提示

**注意：**可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可，你可以返回任意有效的结果。

- 树中的节点数将在 [0, 10^4] 的范围内
- -10^8 <= Node.val <= 10^8
- 所有值 Node.val 是独一无二的
- -10^8 <= val <= 10^8
- 保证 val 在原始的二叉搜索树中不存在



# 题目解析

二叉搜索树的插入操作是一道经典问题，难度中等。

当然这个难度中等是对于没看过我写的二叉搜索树入门文章的同学而言，对于看过的，这道题说实话，洒洒水。

<div align=center>

![701-10](https://cdn.codegoudan.com/img/701-10.png)

</div>

直接放下面了：



[ACM 选手带你玩转二叉搜索树！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475928211&idx=1&sn=822e7de0f29b0926167c02e324f6baa3&chksm=ff22cd1ec85544088377d9aac07dcd3c4e692d0b6046f9f16bdae42b8c887189cf286ce9927e&scene=21#wechat_redirect)



**二叉搜素树的插入操作，其实和二叉树的插入一样，将要插入的节点 node 放在树中合适的位置。**

这个“寻找合适位置”的过程和二叉搜索树的查找操作类似，只不过多了一步“插入”，而且**对于新插入的节点来说，这个“位置”一般都是在叶子节点上**。

具体的步骤就是：

- 如果插入的节点值比根节点的数值小：

- - 如果根节点的左子树为空，那该节点直接插入到根节点的左孩子位置。
  - 如果根节点的左子树不为空，继续遍历左子树寻找插入的位置。

- 如果插入的节点值比根节点的数值大：

- - 如果根节点的右子树为空，那该节点直接插入到根节点的右孩子位置。
  - 如果根节点的右子树不为空，继续遍历右子树寻找插入的位置。

比如对于下图：

<div align=center>

![701-11](https://cdn.codegoudan.com/img/701-11.png)

</div>

我们此时要插入 node = 3 的节点。

第 1 步：根节点的值为 29，node < 29，且节点的左孩子不为空，继续遍历左子树寻找插入的位置。

<div align=center>

![701-12](https://cdn.codegoudan.com/img/701-12.png)

</div>

第 2 步：左子树的根节点值为 4，node < 4，且节点的左孩子不为空，继续遍历左子树寻找插入的位置。

<div align=center>

![701-13](https://cdn.codegoudan.com/img/701-13.png)

</div>

第 3 步：左子树的根节点值为 0，node > 0，且节点的右孩子为空，插入该节点右孩子的位置。

<div align=center>

![701-14](https://cdn.codegoudan.com/img/701-14.png)

</div>



# 递归法

根据【[**递归算法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)】文章中讲的，实现递归，需要两步：

- 找出重复的子问题（递推公式）。
- 终止条件。



**(1) 找出重复的子问题。**

这个其实在上面二叉搜索树的插入步骤中可以看出：

- 如果插入的节点值比根节点的数值小，继续遍历左子树寻找插入的位置。
- 如果插入的节点值比根节点的数值大，继续遍历右子树寻找插入的位置。

对于各个子树也都是同样的操作。

```Python
# 如果根节点的左子树不为空且插入的节点值比根节点值小
if val < root.val:
    # 继续遍历左子树
    self.insertIntoBST(root.left, val)
#  如果根节点的右子树不为空且插入的节点值比根节点值大
if val > root.val:
    # 继续遍历右子树
    self.insertIntoBST(root.right, val)
```



**(2) 确定终止条件。**

对于终止条件就没啥好说的，插入完了就返回。

<div align=center>

![701-15](https://cdn.codegoudan.com/img/701-15.jpg)

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
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        insertNode = TreeNode(val)
        # 如果二叉搜索树为空
        if root == None:
            return insertNode
        # 如果根节点的左子树为空且插入的节点值比根节点值小
        if root.left == None and val < root.val:
            # 该节点插入到根节点的左孩子位置
            root.left = insertNode
        # 如果根节点的右子树为空且插入的节点值比根节点值大
        if root.right == None and val > root.val:
            # 该节点插入到根节点的右孩子位置
            root.right = insertNode
        # 如果根节点的左子树不为空且插入的节点值比根节点值小
        if val < root.val:
            # 继续遍历左子树
            self.insertIntoBST(root.left, val)
        #  如果根节点的右子树不为空且插入的节点值比根节点值大
        if val > root.val:
            # 继续遍历右子树
            self.insertIntoBST(root.right, val)
        # 返回根节点
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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        TreeNode insertNode = new TreeNode(val);
        // 如果二叉搜索树为空
        if(root == null){
            return insertNode;
        }
        // 如果根节点的左子树为空且插入的节点值比根节点值小
        if(root.left == null && val < root.val){
            // 该节点插入到根节点的左孩子位置
            root.left = insertNode;
        }
        // 如果根节点的右子树为空且插入的节点值比根节点值大
        if(root.right == null && val > root.val){
            // 该节点插入到根节点的右孩子位置
            root.right = insertNode;
        }
        // 如果根节点的左子树不为空且插入的节点值比根节点值小
        if(val < root.val){
            // 继续遍历左子树
            insertIntoBST(root.left, val);
        }
        // 如果根节点的右子树不为空且插入的节点值比根节点值大
        if(val > root.val){
            // 继续遍历右子树
            insertIntoBST(root.right, val);
        }
        // 返回根节点
        return root;
    }
}
```



本题解，在最坏情况下二叉搜索树的高度为 n，这时插入在最深的叶子节点上，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉搜索树的高度，同样二叉搜索树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

非递归法的步骤其实就是按照上面“题目解析”中，插入的具体操作步骤即可。

只是要多加一个记录插入节点的父节点 parentNode，通过它来确认插入节点的具体位置。

<div align=center>

![701-16](https://cdn.codegoudan.com/img/701-16.jpg)

</div>

我们以下图为例：

<div align=center>

![701-4](https://cdn.codegoudan.com/img/701-4.png)

</div>

当插入 val = 5 的节点时，让我们一起来看一下它是怎么操作的。

首先初始化存储插入节点父节点的 parentNode 和当前节点 currentNode。

<div align=center>

![701-5](https://cdn.codegoudan.com/img/701-5.png)

</div>

```Python
# 存储插入节点的父节点
parentNode = None
currentNode = root
```



第 1 步，val = 5，currentNode.val = 4，val > currentNode，所以证明 val 要插入右子树，所以让 parentNode 指向节点 4，currentNode 指向节点 7（右孩子）。

<div align=center>

![701-6](https://cdn.codegoudan.com/img/701-6.png)

</div>

```Python
if val > currentNode.val:
    parentNode = currentNode
    currentNode = currentNode.right
```



第 2 步，val = 5，currentNode.val = 7，val < currentNode，所以证明 val 要插入左子树，所以让 parentNode 指向节点 7，currentNode 指向空（左孩子）。

<div align=center>

![701-7](https://cdn.codegoudan.com/img/701-7.png)

</div>

```Python
if val < currentNode.val:
    parentNode = currentNode
    currentNode = currentNode.left
```



第 3 步，val = 5，currentNode.val 指向空，证明找到了要插入的节点的父节点，此时 parentNode.val = 7，val < parentNode.val，这意味着要节点插入 parentNode 的左孩子位置。

<div align=center>

![701-8](https://cdn.codegoudan.com/img/701-8.png)

</div>



```Python
if val < parentNode.val:
    parentNode.left = insertNode
```



至此二叉搜索树中的插入操作结束。

<div align=center>

![701-17](https://cdn.codegoudan.com/img/701-17.jpg)

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
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        insertNode = TreeNode(val)
        # 如果二叉搜索树为空
        if root == None:
            return insertNode
        # 存储插入节点的父节点
        parentNode = None
        currentNode = root
        # 寻找插入节点的父节点
        while currentNode:
            if val < currentNode.val:
                parentNode = currentNode
                currentNode = currentNode.left
            else:
                parentNode = currentNode
                currentNode = currentNode.right
        # 如果插入节点的值比父节点的值小，则插入左子树
        if val < parentNode.val:
            parentNode.left = insertNode
        # 如果插入节点的值比父节点的值小，则插入右子树
        else:
            parentNode.right = insertNode

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
    public TreeNode insertIntoBST(TreeNode root, int val) {
        TreeNode insertNode = new TreeNode(val);
        // 如果二叉搜索树为空
        if(root == null){
            return insertNode;
        }
        // 存储插入节点的父节点
        TreeNode parentNode = null;
        TreeNode currentNode = root;
        // 寻找插入节点的父节点
        while(currentNode != null){
            if(val < currentNode.val){
                parentNode = currentNode;
                currentNode = currentNode.left;
            }
            else{
                parentNode = currentNode;
                currentNode = currentNode.right;
            }
        }
        // 如果插入节点的值比父节点的值小，则插入左子树
        if(val < parentNode.val){
            parentNode.left = insertNode;
        }
        // 如果插入节点的值比父节点的值小，则插入右子树
        else{
            parentNode.right = insertNode;
        }
        return root;
    }
}
```



同样，此解法和递归法一样，**时间复杂度为 O(n)**，但是由于没有使用额外的空间，此时的**空间复杂度为 O(1)**。



---

**图解二叉搜索树中的插入操作**到这就结束辣，到这二叉搜索树的 3 大操作，我们已经玩了【[查找](https://mp.weixin.qq.com/s/tC3RwcEXBCub_FP0KqquSg)】和【插入】，下面当然就要搞最难搞的【删除】，还不赶紧把期待打在留言区~

<div align=center>

![701-18](https://cdn.codegoudan.com/img/701-18.jpg)

</div>

那今天就到这结束辣，我是帅蛋，我们下次见~
