大家好呀，我是快乐的蛋蛋。

今天解决**从中序与后序遍历序列构造二叉树**，和之前的【[**从前序与中序遍历构造二叉树**](https://mp.weixin.qq.com/s/lM4_ntDfpoQRhbeXpCZaKw)】相同，考察小婊贝们对二叉树前中后序遍历的掌握程度。

关于二叉树的前中后序遍历，如果还不太了解，可以看下面这两篇文章：



[**ACM 选手带你玩转二叉树前中后序遍历（递归版）**](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)

[**ACM 选手带你玩转二叉树前中后序遍历（非递归版）**](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



还有，提醒一下，一定要看文末呦。

<div align=center>

![106-0](https://cdn.codegoudan.com/img/106-0.png)

</div>



# LeetCode 106：从中序与后序遍历序列构造二叉树



## 题意

根据一棵树的中序遍历与后序遍历构造二叉树。



## 示例

输入：inorder = [9,3,15,20,7]，postorder = [9,15,7,20,3]。

输出：[3,9,20,null,null,15,7]



## 提示

- 假设树中没有重复元素。



# 题目解析

难度中等，**考察二叉树前中后序遍历掌握程度的经典好题**。

同样对于这道题，如果真的完全理解了前中后序遍历的原理，是不难的。

二叉树的中序遍历顺序为：左子树、根、右子树，所以对于二叉树来说，中序遍历的形式如下图所示：

<div align=center>

![106-1](https://cdn.codegoudan.com/img/106-1.png)

</div>

二叉树的后序遍历顺序为：左子树、右子树、根，所以对于二叉树来说，后序遍历的形式如下图所示：

<div align=center>

![106-2](https://cdn.codegoudan.com/img/106-2.png)

</div>

通过上面两个图，就可以很清晰的看出，中序遍历的根节点左侧是左子树，右侧是右子树，后序遍历的最后一个元素为根节点。

看懂了这个，那解题步骤也就出来了：

**在中序遍历中找到根节点，从而找到左右子树，知道左右子树的范围，从而后序遍历中的左右子树也就确定好了。**

然后分别对左右子树用同样的方式递归构造下去。



# 图解

以 inorder = [9,3,15,20,7]，postorder = [9,15,7,20,3] 为例。

第 1 步，在后序遍历中找到根节点的值，然后创建根节点。

<div align=center>

![106-3](https://cdn.codegoudan.com/img/106-3.png)

</div>

```Python
# 根节点的值为后序遍历的最后一个元素值
rootVal = postorder[-1]
# 创建根节点
root = TreeNode(rootVal)
```

第 2 步，在中序遍历中找到根节点的位置 midIndex。

<div align=center>

![106-4](https://cdn.codegoudan.com/img/106-4.png)

</div>

```Python
# 用根节点的值去中序数组中查找对应元素下标
midIndex = inorder.index(rootVal)
```

如上图所示，中序遍历的根节点下标位置为 1。

第 3 步，找出中序遍历的左右子树。

可以很明显的从上图中看出：

- 中序遍历的左子树范围为**[0,midIndex - 1]**。
- 中序遍历的右子树的范围为**[midIndex + 1,n - 1]**。

<div align=center>

![106-5](https://cdn.codegoudan.com/img/106-5.png)

</div>

```Python
# 找出中序遍历的左子树和右子树
# 左子树区间为 [0,midIndex),右子树区间为[midIndex+1，n-1]
inorderLeft = inorder[:midIndex]
inorderRight = inorder[midIndex + 1:]
```

第 4 步，找出后序遍历的左右子树。

通过第 3 步得出，**中序遍历左子树长度为 1，右子树长度为 3，所以对于后序遍历来说，其左子树长度同样为 1，右子树长度同样为 3**。

这样就可以在后序遍历中确定其左右子树的范围：

- 后序遍历左子树的范围为 **[0,len(inorderLeft))**。
- 后序遍历右子树的范围为 **[len(inorderLeft), n - 1)**。

<div align=center>

![106-6](https://cdn.codegoudan.com/img/106-6.png)

</div>

```Python
# 找出后序遍历的左子树和右子树
# 后序遍历和中序遍历的左子树和右子树长度相等，所以可以通过中序遍历左右子树的长度来确定后序遍历左右子树的位置
postorderLeft = postorder[: len(inorderLeft)]
postorderRight = postorder[len(inorderLeft): len(inorder) - 1]
```

接下来，以同样的方式递归遍历左子树和右子树，直至结束。

<div align=center>

![106-7](https://cdn.codegoudan.com/img/106-7.png)

</div>

```Python
# 递归遍历左子树
root.left = self.buildTree(inorderLeft, postorderLeft)
# 递归遍历右子树
root.right = self.buildTree(inorderRight, postorderRight)
```

本题解每个数组的元素遍历一次，**时间复杂度为 O(n)**。

此外结果输出一个数组，**空间复杂度 O(n)**。



# 代码实现



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        # 递归中止条件：树为空
        if not inorder or not postorder:
            return None

        # 根节点的值为后序遍历的最后一个元素值
        rootVal = postorder[-1]
        # 创建根节点
        root = TreeNode(rootVal)

        # 用根节点的值去中序数组中查找对应元素下标
        midIndex = inorder.index(rootVal)

        # 找出中序遍历的左子树和右子树
        # 左子树区间为 [0,midIndex),右子树区间为[midIndex+1，n-1]
        inorderLeft = inorder[:midIndex]
        inorderRight = inorder[midIndex + 1:]

        # 找出后序遍历的左子树和右子树
        # 后序遍历和中序遍历的左子树和右子树长度相等，所以可以通过中序遍历左右子树的长度来确定后序遍历左右子树的位置
        postorderLeft = postorder[: len(inorderLeft)]
        postorderRight = postorder[len(inorderLeft): len(inorder) - 1]

        # 递归遍历左子树
        root.left = self.buildTree(inorderLeft, postorderLeft)
        # 递归遍历右子树
        root.right = self.buildTree(inorderRight, postorderRight)

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
  public TreeNode buildTree(int[] inorder, int[] postorder) {
        // 递归中止条件：树为空
    if(inorder.length == 0 || postorder.length == 0) {
      return null;
    }
    // 根节点的值为前序遍历的第一个元素值
    // 创建根节点
    TreeNode root = new TreeNode(postorder[postorder.length - 1]);

    for(int i=0 ;i<inorder.length; ++i) {
      //  # 用根节点的值去中序数组中查找对应元素下标
      if(postorder[postorder.length - 1] == inorder[i]) {
        // 找出中序遍历的左子树和右子树
        int[] inorder_left = Arrays.copyOfRange(inorder,0,i);
        int[] inorder_right = Arrays.copyOfRange(inorder,i+1,inorder.length);
        // 找出后序遍历的左子树和右子树
        int[] postorder_left = Arrays.copyOfRange(postorder,0,i);
        int[] postorder_right = Arrays.copyOfRange(postorder,i,postorder.length - 1);
        // 递归遍历左子树
        root.left = buildTree(inorder_left,postorder_left);
        // 递归遍历右子树
        root.right = buildTree(inorder_right,postorder_right);
        break;
      }
    }
    return root;
  }
}
```



---

**图解从中序与后序遍历序列构造二叉树**到这就结束辣，如果你【[**从前序与中序遍历构造二叉树**](https://mp.weixin.qq.com/s/lM4_ntDfpoQRhbeXpCZaKw)】有认真做。就会发现，这两道题差不多，只是稍微做了些改变。

聪明的你可能猜出下一道题该是：从前序与后序遍历序列构造二叉树。

哈哈哈哈...可惜猜错了：**前序与后序遍历序列无法确定一棵二叉树**！

<div align=center>

![106-8](https://cdn.codegoudan.com/img/106-8.jpg)

</div>

原因很简单。

因为前序遍历的根节点在最前面，后序遍历的根节点在最后面，根节点倒是好确定，但是左右子树就不知道谁是谁了。

这一点希望大家记住。当然还得记住本帅蛋的点赞呀~

我是帅蛋，下次让我见到更优秀的你呦~
