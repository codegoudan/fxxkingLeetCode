今天解决**验证二叉搜索树**，通过这道题，我们来看如何证明一棵二叉树是二叉搜索树。

碰到这种问题不要慌，从二叉搜索树性质入手，保准可以找到解决办法。

下面让我们一起来解决掉它！

<div align=center>

![0](https://cdn.codegoudan.com/img/0.png)

</div>



#  LeetCode 98：验证二叉搜索树



## 题意

给你一个二叉树的根节点 root，判断其是否是一个有效的二叉搜索树。



## 示例

输入：root = [2,1,3]
输出：true

<div align=center>

![1](https://cdn.codegoudan.com/img/1.png)

</div>



## 提示

- 树中节点数目范围在 [1,10^4] 内。
- -2^31 <= Node.val <= 2^31 - 1。



# 题目解析

二叉搜索树的经典题目，难度中等。

验证一棵二叉树是不是二叉搜索树，其实就看它符不符合二叉搜索树的性质，符合那就是二叉搜索树，不符合那就不是。

<div align=center>

![bq1](https://cdn.codegoudan.com/img/bq1.jpg)

</div>

是我在【[**二叉搜索树**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475928211&idx=1&sn=822e7de0f29b0926167c02e324f6baa3&chksm=ff22cd1ec85544088377d9aac07dcd3c4e692d0b6046f9f16bdae42b8c887189cf286ce9927e&scene=21#wechat_redirect)】的文章中写过，二叉搜索树拥有一个性质：**对二叉搜索树进行中序遍历时，得到的结果是一个有序的序列**。

比如对于下面这张图：

<div align=center>

![9](https://cdn.codegoudan.com/img/9.png)

</div>

最左侧二叉搜索树遍历结果是：

<div align=center>

![10](https://cdn.codegoudan.com/img/10.png)

</div>

所以，解决这道题其实就成了：

验证对应二叉搜索树中序遍历后的序列是否为有序数列，如果有序，则是二叉搜索树。

<div align=center>

![bq2](https://cdn.codegoudan.com/img/bq2.jpg)

</div>



# 递归法

验证二叉搜索树的递归法其实就是二叉树中序遍历的递归法。



[ACM 选手带你玩转二叉树前中后序遍历（递归版）](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)



但凡是递归我们就分为两步来看：



**(1) 找出重复的子问题。**

中序遍历的顺序是：左子树、根、右子树。

对于左子树、右子树来说，也是同样的遍历顺序。

所以这个重复的子问题就是：**先遍历左子树、再取根节点，最后遍历右子树**。

```Python
inOrder(root.left)
print(root.val)
inOrder(root.right)
```



**(2) 确定终止条件。**

和前序遍历相同，就是当前的节点为空，空的没啥好遍历的。

```Python
if root == None:
    return 
```



最重要的两点确定完了，然后代码基本上就出来。

**最后记得在主函数中做有序序列的判断即可。**



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # 中序遍历
    def inOrder(self, root: TreeNode, res):
        if root == None:
            return
        self.inOrder(root.left, res)
        res.append(root.val)
        self.inOrder(root.right, res)


    def isValidBST(self, root: TreeNode) -> bool:
        res = []
        self.inOrder(root, res)
        # 判断 res 是否有序
        for i in range(1, len(res)):
            if num[i] <= nums[i - 1]:
                return False
        return True
```



## Java 代码实现

```Java
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # 中序遍历
    def inOrder(self, root: TreeNode, res):
        if root == None:
            return
        self.inOrder(root.left, res)
        res.append(root.val)
        self.inOrder(root.right, res)


    def isValidBST(self, root: TreeNode) -> bool:
        res = []
        self.inOrder(root, res)
        # 判断 res 是否有序
        for i in range(1, len(res)):
            if num[i] <= nums[i - 1]:
                return False
        return True
```



此解法，由于每个节点被遍历一次，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，维护了一个 res 的结果数组，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

对于中序遍历而言，**访问节点的顺序和处理节点的顺序是不一致的**，并且，**处理节点是在遍历完左子树之后**。

直白点就是：**从根节点开始，一层层的遍历，找到左子树最左的那个节点，从它开始处理节点。**

例如下图中的 H 节点。

<div align=center>

![11](https://cdn.codegoudan.com/img/11.png)

</div>

常规的**中序遍历具体步骤**如下所示：

- 初始化一个空栈。

- 当【根节点不为空】或者【栈不为空】时，从根节点开始

- - 若当前节点有左子树，一直遍历左子树，每次将当前节点压入栈中。
  - 若当前节点无左子树，从栈中弹出该节点，尝试访问该节点的右子树。

在我们这道题中，我们还需要判断【序列是否有序】，这就需要我们在从栈中弹出节点的时候，和上一次弹出的节点值作比较，如果当前的值大，那就继续之前的操作，否则就证明不是二叉搜索树。

以下图为例：

<div align=center>

![2](https://cdn.codegoudan.com/img/2.png)

</div>

首先初始化一个空栈 stack 和一个保存前一个节点的 pre。

<div align=center>

![3](https://cdn.codegoudan.com/img/3.png)

</div>

```Python
stack = []
# 记录前一个节点
pre = None
```

从根节点开始，一直向左子树遍历，同时将当前的节点压入栈中。

<div align=center>

![4](https://cdn.codegoudan.com/img/4.png)

</div>

```Python
# 一直向左子树走，每一次将当前节点保存到栈中
if root:
    stack.append(root)
    root = root.left
```

当前走到了最左面，弹出栈顶元素，此时 cur = 1，pre 为空，让 pre = cur。

<div align=center>

![5](https://cdn.codegoudan.com/img/5.png)

</div>

```Python
cur = stack.pop()
# 判断序列是否有序
if pre and cur.val <= pre.val:
    return False
pre = cur
root = cur.right
```

弹出的节点 1 并无右子树，继续重复上述的动作。

弹出栈顶元素，此时 cur = 2，pre = 1，cur > pre，证明当前有序。

<div align=center>

![6](https://cdn.codegoudan.com/img/6.png)

</div>

此时让 pre = cur，同时当前的节点 2 有右子树，遍历其右子树，遍历到的节点入栈。

<div align=center>

![7](https://cdn.codegoudan.com/img/7.png)

</div>

弹出栈顶元素，此时 cur = 3，pre = 2，cur > pre，证明当前有序。

<div align=center>

![8](https://cdn.codegoudan.com/img/8.png)

</div>

此时让 pre = cur，同时弹出的节点 3 并无右子树，至此二叉树全部遍历完，返回 True。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        stack = []
        # 记录前一个节点
        pre = None

        while root or stack:
            # 一直向左子树走，每一次将当前节点保存到栈中
            if root:
                stack.append(root)
                root = root.left
            # 当前节点为空，证明走到了最左边，从栈中弹出节点
            # 开始对右子树重复上述过程
            else:
                cur = stack.pop()
                # 判断序列是否有序
                if pre and cur.val <= pre.val:
                    return False
                pre = cur
                root = cur.right

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
    public boolean isValidBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        // 记录前一个节点
        TreeNode pre = null;
        
        while(stack.size() > 0 || root != null){
            // 一直向左子树走，每一次将当前节点保存到栈中
            if(root != null){
                stack.add(root);
                root = root.left;
            }
            // 当前节点为空，证明走到了最左边，从栈中弹出节点
            // 开始对右子树重复上述过程
            else{
                TreeNode cur = stack.pop();
                // 判断序列是否有序
                if(pre != null && cur.val <= pre.val){
                    return false;
                }
                pre = cur;
                root = cur.right;
            }
        }
        return true;
    }
}
```



同样，非递归版的解法**时间复杂度为O(n)，空间复杂度为 O(n)**。



---

**图解验证二叉搜索树**到这就结束了，你看，虽然是个难度中等的题，也只是个纸老虎。

掌握了二叉搜索树的性质就掌握了这道题的解题密码，其实这道题也给我们提供了思路，有时候可以往本身的性质上靠一下。

