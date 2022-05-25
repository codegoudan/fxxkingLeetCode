大家好呀，我是帅蛋。

今天解决**从前序与中序遍历构造二叉树**，这种题目就是为了考察小婊贝们对二叉树前中后序遍历的掌握程度。

真正理解了它们的原理，解决起来是不难的。

关于二叉树的前中后序遍历，如果还不太了解，可以看下面这两篇文章：



[ACM 选手带你玩转二叉树前中后序遍历（递归版）](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)

[ACM 选手带你玩转二叉树前中后序遍历（非递归版）](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



那咱话不多说，开整！

<div align=center>

![105-0](https://cdn.codegoudan.com/img/105-0.png)

</div>



# LeetCode 105：从前序与中序遍历序列构造二叉树



## 题意

给定一棵树的前序遍历 preorder 与中序遍历 inorder。请构造二叉树并返回其根节点。



## 示例

输入：preorder = [3,9,20,15,7]，inorder = [9,3,15,20,7]

输出：[3,9,20,null,null,15,7]



## 提示

- 1 <= len(preorder) == len(inorder) <= 3000
- -3000 <= preorder[i]，inorder[i] <= 3000
- preorder 和 inorder 均无重复元素
- inorder 均出现在 preorder
- preorder 保证为二叉树的前序遍历序列
- inorder 保证为二叉树的中序遍历序列



# 题目解析

难度中等，**考察二叉树前中后序遍历掌握程度的经典好题**。

对于这种题目，如果真的完全理解了前中后序遍历的原理，是不难的。

二叉树前序遍历的顺序为：根、左子树、右子树，所以对于二叉树来说，前序遍历的形式如下图所示：

<div align=center>

![105-1](https://cdn.codegoudan.com/img/105-1.png)

</div>

二叉树的中序遍历顺序为：左子树、根、右子树，所以对于二叉树来说，中序遍历的形式如下图所示：

<div align=center>

![105-2](https://cdn.codegoudan.com/img/105-2.png)

</div>

通过上面两个图，就可以很清晰的看出，前序遍历的第一个元素为根节点，中序遍历的根节点左侧是左子树，右侧是右子树。

知道了这些，解题思路呼之欲出，即：

**在中序遍历中找到根节点，从而找到左右子树，知道左右子树的范围，从而前序遍历中的左右子树也就确定好了。**

然后分别对左右子树重复这个方式，直至结束。



# 图解

以 preorder = [3,9,20,15,7]，inorder = [9,3,15,20,7] 为例。

第 1 步，在前序遍历中找到根节点的值，然后创建根节点。

<div align=center>

![105-3](https://cdn.codegoudan.com/img/105-3.png)

</div>

```Python
# 根节点的值为前序遍历的第一个元素值
rootVal = preorder[0]
# 创建根节点
root = TreeNode(rootVal)
```

第 2 步，在中序遍历中找到根节点的位置 midIndex。

<div align=center>

![105-4](https://cdn.codegoudan.com/img/105-4.png)

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

![105-5](https://cdn.codegoudan.com/img/105-5.png)

</div>

```Python
# 找出中序遍历的左子树和右子树
# 左子树区间为 [0,midIndex),右子树区间为[midIndex+1，n-1]
inorderLeft = inorder[:midIndex]
inorderRight = inorder[midIndex + 1:]
```

第 4 步，找出前序遍历的左右子树。

通过第 3 步得出，**中序遍历左子树长度为 1，右子树长度为 3，所以对于前序遍历来说，其左子树长度同样为 1，右子树长度同样为 3**。

这样就可以在前序遍历中确定其左右子树的范围：

- 前序遍历左子树的范围为 **[1,len(inorderLeft) + 1)**。
- 前序遍历右子树的范围为 **[len(inorderLeft) + 1, n)**。

<div align=center>

![105-6](https://cdn.codegoudan.com/img/105-6.png)

</div>

```Python
# 找出前序遍历的左子树和右子树
# 前序遍历和中序遍历的左子树和右子树长度相等，所以可以通过中序遍历左右子树的长度来确定前序遍历左右子树的位置
preorderLeft = preorder[1:len(inorderLeft) + 1]
preorderRight = preorder[len(inorderLeft) + 1:]
```

接下来，以同样的方式递归遍历左子树和右子树，直至结束。

<div align=center>

![105-7](https://cdn.codegoudan.com/img/105-7.png)

</div>

```Python
# 递归遍历左子树
root.left = self.buildTree(preorderLeft, inorderLeft)
# 递归遍历右子树
root.right = self.buildTree(preorderRight, inorderRight)
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
      def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        # 递归中止条件：树为空
        if not preorder or not inorder:
            return None
  
        # 根节点的值为前序遍历的第一个元素值
        rootVal = preorder[0]
        # 创建根节点
        root = TreeNode(rootVal)
  
        # 用根节点的值去中序数组中查找对应元素下标
        midIndex = inorder.index(rootVal)
  
        # 找出中序遍历的左子树和右子树
        # 左子树区间为 [0,midIndex),右子树区间为[midIndex+1，n-1]
        inorderLeft = inorder[:midIndex]
        inorderRight = inorder[midIndex + 1:]
  
        # 找出前序遍历的左子树和右子树
        # 前序遍历和中序遍历的左子树和右子树长度相等，所以可以通过中序遍历左右子树的长度来确定前序遍历左右子树的位置
        preorderLeft = preorder[1:len(inorderLeft) + 1]
        preorderRight = preorder[len(inorderLeft) + 1:]
  
        # 递归遍历左子树
        root.left = self.buildTree(preorderLeft, inorderLeft)
        # 递归遍历右子树
        root.right = self.buildTree(preorderRight, inorderRight)
  
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
  public TreeNode buildTree(int[] preorder, int[] inorder) {
        // 递归中止条件：树为空
    if(preorder.length==0 || inorder.length==0) {
      return null;
    }
    // 根节点的值为前序遍历的第一个元素值
        // 创建根节点
    TreeNode root = new TreeNode(preorder[0]);

    for(int i=0 ;i<preorder.length; ++i) {
      //  # 用根节点的值去中序数组中查找对应元素下标
      if(preorder[0]==inorder[i]) {
                // 找出中序遍历的左子树和右子树
                int[] inorder_left = Arrays.copyOfRange(inorder,0,i);
        int[] inorder_right = Arrays.copyOfRange(inorder,i+1,inorder.length);
                // 找出前序遍历的左子树和右子树
        int[] preorder_left = Arrays.copyOfRange(preorder,1,i+1);
        int[] preorder_right = Arrays.copyOfRange(preorder,i+1,preorder.length);
                // 递归遍历左子树
        root.left = buildTree(preorder_left,inorder_left);
                // 递归遍历右子树
        root.right = buildTree(preorder_right,inorder_right);
        break;
      }
    }
    return root;
  }
}
```



---

**图解从前序遍历与中序遍历构造二叉树**到这就结束辣，是不是觉得还挺简单？

还是那句话，原理搞懂了，解题那是分分钟钟的事。

如果分分钟写不出来代码，那是编程能力的问题，得多想多练。

怎么练？可以先从给本蛋**点赞**开始。

我是帅蛋，下次见的时候你一定比现在进步了哟~
