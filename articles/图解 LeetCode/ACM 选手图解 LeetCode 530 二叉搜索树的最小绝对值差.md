大家好呀，我是帅蛋。

今天解决**二叉搜索树的最小绝对差**，这道题看着名字挺唬人，本质上还是和【[**验证二叉搜索树**](https://mp.weixin.qq.com/s/I8kEYkfzLe7Tdqbr9Nqang)】一样，从二叉树搜索树的性质入手，一击必中。

话不多说，直接开整！

<div align=center>

![530-0](https://cdn.codegoudan.com/img/530-0.png)

</div>



# LeetCode 530：二叉搜索树的最小绝对差

题目链接：[二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)



## 题意

给你一个二叉搜索树的根节点 root，返回树中任意两不同节点值之间的最小差值。

差值是一个正数，其数值等于两值只差的绝对值。



## 示例

输入：root = [4,2,6,1,3]
输出：1

<div align=center>

![530-1](https://cdn.codegoudan.com/img/530-1.png)

</div>



## 提示

- 树中节点的数目范围是 [2, 10^4]。
- 0 <= Node.val <= 10^5



# 题目解析

二叉搜索树的最小绝对值差，难度简单，我觉得这个难度评级才算的上它的归宿。

<div align=center>

![530-8](https://cdn.codegoudan.com/img/530-8.png)

</div>

其实这道题和我们之前做过的【[**验证二叉搜索树**](https://mp.weixin.qq.com/s/I8kEYkfzLe7Tdqbr9Nqang)】的解法是一样的，用到的也还是**二叉搜索树进行中序遍历时，得到的结果是一个有序的序列**这个性质。

如果还不清楚二叉搜索树的，可以看我下面这篇文章：



[ACM 选手带你玩转二叉搜索树！](https://mp.weixin.qq.com/s/DsWb4oXeOWsCRiuPRwJysg)



所以中遍历后，这道题就变成了“在一个有序序列中找最小绝对差”这种难度为 0 的题目。

<div align=center>

![530-9](https://cdn.codegoudan.com/img/530-9.jpg)

</div>



# 递归版

根据上面讲的，那二叉搜索树的最小绝对差的递归法其实就是二叉树中序遍历的递归法。



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

和中序遍历相同，就是当前的节点为空，空的没啥好遍历的。

```python
if root == None:
    return 
```



最重要的两点确定完了，然后代码基本上就出来。

**最后记得在主函数中做最小绝对差判断即可。**



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

    def getMinimumDifference(self, root: TreeNode) -> int:
        res = []
        minRes = float('inf')
        self.inOrder(root, res)
        # 求最小绝对差
        for i in range(len(res) - 1):
            minRes = min(abs(res[i] - res[i + 1]), minRes)

        return minRes
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
    // 中序遍历
    public void inOrder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        inOrder(root.left, res);
        res.add(root.val);
        inOrder(root.right, res);
    }

    public int getMinimumDifference(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        int minRes = Integer.MAX_VALUE;
        inOrder(root, res);
        // 求最小绝对差
        for(int i = 0; i < res.size() - 1; i++){
            minRes = Math.min(res.get(i+1) - res.get(i), minRes);
        }
        return minRes;
    }
}
```



此解法，中序遍历每个节点被遍历一次，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，维护了一个 res 的结果数组，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

对于中序遍历而言，**处理节点是在遍历完左子树之后**。

直白点就是：**从根节点开始，一层层的遍历，找到左子树最左的那个节点，从它开始处理节点。**

如果对非递归的二叉树中序遍历不熟悉的同学，可以看下面这篇文章：



[ACM 选手带你玩转二叉树前中后序遍历（非递归版）](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



常规的**中序遍历具体步骤**如下所示：

- 初始化一个空栈。

- 当【根节点不为空】或者【栈不为空】时，从根节点开始

- - 若当前节点有左子树，一直遍历左子树，每次将当前节点压入栈中。
  - 若当前节点无左子树，从栈中弹出该节点，尝试访问该节点的右子树。

在我们这道题中，我们还需要判断【有序序列的最小绝对差】。

对于有序序列来说，求任意的两个元素之差的绝对值的最小值，一定就是两个相邻元素差的最小值。

这就需要我们在从栈中弹出节点的时候，用当前节点值减去上一次弹出的节点值，用来与 minRes 比较，维护最小值 minRes。

<div align=center>

![530-10](https://cdn.codegoudan.com/img/530-10.jpg)

</div>

以下图为例：

<div align=center>

![530-2](https://cdn.codegoudan.com/img/530-2.png)

</div>

首先初始化一个空栈 stack、一个记录最小值 minRes 和一个保存前一个节点的 pre。

<div align=center>

![530-3](https://cdn.codegoudan.com/img/530-3.png)

</div>

```Python
stack = []
# 记录最小值
minRes = float('inf')
# 记录前一个节点
pre = None
```



从根节点开始，一直向左子树遍历，同时将当前的节点压入栈中。

<div align=center>

![530-4](https://cdn.codegoudan.com/img/530-4.png)

</div>

```Python
# 一直向左子树走，每一次将当前节点保存到栈中
if root:
    stack.append(root)
    root = root.left
```



当前走到了最左面，弹出栈顶元素，此时 cur = 1，pre 为空，让 pre = cur。

<div align=center>

![530-5](https://cdn.codegoudan.com/img/530-5.png)

</div>

```Python
cur = stack.pop()
# 求最小绝对差
if pre:
    minRes = min(cur.val - pre.val, minRes)
pre = cur
root = cur.right
```



弹出的节点 1 无右子树，继续重复上述的动作。

弹出栈顶元素，此时 cur  = 2，pre = 1，minRes = inf，此时 pre 不为空，minRes = min(cur.val - pre.val, minRes) = min(1, inf) = 1。

<div align=center>

![530-6](https://cdn.codegoudan.com/img/530-6.png)

</div>

此时让 pre = cur，同时当前的节点 2 有右子树，遍历其右子树，遍历到的节点入栈。

<div align=center>

![530-7](https://cdn.codegoudan.com/img/530-7.png)

</div>

之后的步骤其实就和上面一样，不断出栈、入栈，维护 pre 和 minRes 的值，直至栈为空。

最后返回 minRes = 1。

<div align=center>

![530-11](https://cdn.codegoudan.com/img/530-11.jpg)

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
    def getMinimumDifference(self, root: TreeNode) -> int:
        stack = []
        # 记录最小值
        minRes = float('inf')
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
                # 求最小绝对差
                if pre:
                    minRes = min(cur.val - pre.val, minRes)
                pre = cur
                root = cur.right
        return minRes
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
    public int getMinimumDifference(TreeNode root) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        // 记录最小值
        int minRes = Integer.MAX_VALUE;
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
                if(pre != null){
                    minRes = Math.min(cur.val - pre.val, minRes);
                }
                pre = cur;
                root = cur.right;
            }
        }
        return minRes;
    }
}
```



同样，非递归版的解法**时间复杂度为** **O(n)，空间复杂度为 O(n)**。



---

**图解二叉搜索树的最小绝对差**到这就结束辣，又搞定了一道题，是不是贼开心？

还是那句话，掌握了二叉搜索树的性质就掌握了解题密码，真是有意思的二叉搜索树！

<div align=center>

![530-12](https://cdn.codegoudan.com/img/530-12.jpg)

</div>

今天就到这了，我是帅蛋，我们下次见！
