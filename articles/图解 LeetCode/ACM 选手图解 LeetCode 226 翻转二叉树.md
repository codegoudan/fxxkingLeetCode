大家好呀，我是翻转蛋。

今天解决**翻转二叉树**，这是一道看起来很简单的题目，当然做起来也简单。

但是作为一道经典的二叉树题目，这道题存在的意义不只是为了 AC 那一下的快感。

而是要**把它和之前所学的内容连起来，你要知道你用了什么方法去解决**，继续看接下来的内容，你们就知道我在说什么。

<div align=center>

![226-0](https://cdn.codegoudan.com/img/226-0.png)

</div>



# LeetCode 226：翻转二叉树



## 题意

翻转一颗二叉树。



## 示例

输入：

<div align=center>

![226-1](https://cdn.codegoudan.com/img/226-1.png)

</div>

输出：

<div align=center>

![226-2](https://cdn.codegoudan.com/img/226-2.png)

</div>



## 提示

- 爱帅蛋
- 么么哒



# 题目解析

题目经典，难度简单。

**翻转二叉树，就是将每个节点的左右孩子进行交换。**

这个说白了就是分解成了多个重复的子问题，显然可以用递归法来解决。

其实我们在做二叉树相关题目的时候，脑子里的第一反应出来的应该是用递归解决。

再回到“交换”本身，根据“交换”的顺序不同，这道题其实是有 **3 种解法**：

**(1) 由上至下**

由上至下，就是**先交换左右子树，再递归左子树，右子树**。

这样就是，先换的是根节点的左右子树，剩下内部的子树还没换，那就交给递归去换。

<div align=center>

![226-3](https://cdn.codegoudan.com/img/226-3.png)

</div>



**(2) 由下至上**

由下至上，就是**先递归左子树，右子树，再交换左右**。

这就意味着先从叶子节点的交换开始，然后随着递归向上，子树被一个个的翻转。

<div align=center>

![226-4](https://cdn.codegoudan.com/img/226-4.png)

</div>



**(3) 由左至右**

由左至右，就是**从根节点开始，一层层的遍历，一层层的换**。

<div align=center>

![226-5](https://cdn.codegoudan.com/img/226-5.png)

</div>



看完这三种方式，不知道小婊贝们看懂了没...

<div align=center>

![226-6](https://cdn.codegoudan.com/img/226-6.gif)

</div>

**由上至下对应着前序遍历，由下至上对应着后序遍历，由左至右对应着层次遍历。**

可能这里有小婊贝好奇：**为啥少了个中序遍历？**



> good question！
>
> 
>
> 其实这道题用**中序遍历是可以的，但是会有点怪，不是严格意义上我们说的中序遍历的样子**。
>
> 
>
> 这个是因为中序遍历的顺序是：左子树、根、右子树，应用在这道题上，如果仿照前序和后序的样子，应该是递归左子树、交换左右子树、递归右子树。
>
> 
>
> 但是这里不能这么做，因为交换了左右子树后，左右子树已经换了位置。递归右子树，其实就是在递归之前的左子树。
>
> 
>
> 所以**想用中序遍历，递归法的顺序应该是：递归左子树、交换左右子树、递归左子树。**



如果对这几个遍历不熟悉的，可以看下面这几篇文章：



[ACM 选手带你玩转二叉树前中后序遍历（递归版）](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)



[ACM 选手带你玩转二叉树前中后序遍历（非递归版）](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



[ACM 选手带你玩转二叉树层次遍历（递归 + 非递归）](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)



既然能用递归解决，那就肯定也存在非递归的方法（迭代）。

这样看来，**其实本题有 4 种解法，用二叉树的前中后序+层次遍历都可以解决**。



# 递归法

因为篇幅原因，这里我以前序遍历为例，解决本题。

根据【[**递归算法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)】文章中讲的，实现递归，需要两步：

- 找出重复的子问题（递推公式）。
- 终止条件。

根据上面讲的实现递归的两步来实现：

**(1) 找出重复的子问题。**

这个很好找，前序遍历的顺序是：根、左子树、右子树。

对应到本题是：交换左右子树，左子树，右子树。

对于左子树或者右子树来说，也是同样的操作顺序。

所以这个重复的子问题就出来了，**先交换左右子树，再遍历左子树，最后遍历右子树**。

```Python
# 将当前节点的左右子树交换
root.left, root.right = root.right, root.left
# 递归左子树
self.invertTree(root.left)
# 递归右子树
self.invertTree(root.right)
```



**(2) 确定终止条件。**

对于二叉树的遍历来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

那就是当前的节点是空的，既然是空的那就没啥好遍历。

```Python
# 递归终止条件
if root == None:
return None
```



这两点确定好了，代码也就出来了。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        # 递归终止条件
        if root == None:
            return None
        
        # 将当前节点的左右子树交换
        root.left, root.right = root.right, root.left
        
        # 递归左子树
        self.invertTree(root.left)
        # 递归右子树
        self.invertTree(root.right)
        
        return root
```



## Java 代码实现

```Java
class Solution {
	public TreeNode invertTree(TreeNode root) {
		//递归终止条件
		if(root == null) {
			return null;
		}
		//将当前节点的左右子树交换
		TreeNode tmp = root.right;
		root.right = root.left;
		root.left = tmp;
        
		//递归左子树
		invertTree(root.left);
		//递归右子树
		invertTree(root.right);

		return root;
	}
}
```



此解法，由于每个节点被遍历一次，且节点间进行了交换，所以**时间复杂度为 O(n)**。

使用递归，在过程中额外调用了栈空间，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

同样还是因为篇幅原因，这里我以前序遍历为例。

我在上面递归法中说过，对于本题的顺序是：**先交换左右子树，再遍历左子树，最后遍历右子树**。

因为栈“先入后出”的特点，结合上述的顺序，迭代的过程也就出来了：

**每次都是先将根节点放入栈，然后右子树，最后左子树。**

**具体步骤**如下所示：

- 初始化维护一个栈，将根节点入栈。

- 当栈不为空时

- - 弹出栈顶元素 node，将栈顶元素 node 的左右子树交换。
  - 若 node 的右子树不为空，右子树入栈。
  - 若 node 的左子树不为空，左子树入栈。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
# class Solution:
#     def invertTree(self, root: TreeNode) -> TreeNode:
#         # 递归终止条件
#         if root == None:
#             return None
        
#         # 将当前节点的左右子树交换
#         root.left, root.right = root.right, root.left
        
#         # 递归左子树
#         self.invertTree(root.left)
#         # 递归右子树
#         self.invertTree(root.right)
        
#         return root

class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if root == None:
            return None
        
        stack = [root]
        
        while stack:
            # 当前节点出栈
            node = stack.pop()
            # 将当前节点的左右子树交换
            node.left, node.right = node.right, node.left
            
            # 右子树入栈
            if node.right:
                stack.append(node.right)
            # 左子树入栈
            if node.left:
                stack.append(node.left)
            
        return root
```



## Java 代码实现

```Java
// class Solution {
// 	public TreeNode invertTree(TreeNode root) {
// 		//递归终止条件
// 		if(root == null) {
// 			return null;
// 		}
// 		//将当前节点的左右子树交换
// 		TreeNode tmp = root.right;
// 		root.right = root.left;
// 		root.left = tmp;
        
// 		//递归左子树
// 		invertTree(root.left);
// 		//递归右子树
// 		invertTree(root.right);

// 		return root;
// 	}
// }

class Solution {
	public TreeNode invertTree(TreeNode root) {
		//递归终止条件
		if(root == null) {
			return null;
		}
     Stack<TreeNode> stack = new Stack<>();
      stack.push(root);
      ArrayList<Integer> res = new ArrayList<>();
      
      while(!stack.isEmpty()) {
        //当前节点出栈
        TreeNode node = stack.pop();
        //将当前节点的左右子树交换
		TreeNode tmp = node.right;
		node.right = node.left;
		node.left = tmp;
        //右子树入栈
        if(node.right != null) {
          stack.push(node.right);
        }
        //左子树入栈
        if(node.left != null) {
          stack.push(node.left);
        }
      }
      return root;
	}
}
```



同样非递归法，**时间复杂度为 O(n)，空间复杂度为 O(n)**。



---

**图解翻转二叉树**到这就结束辣，你学会了嘛？！

本来是个翻转二叉树，谁刚开始能想到翻转能和遍历扯上关系，扯上就算了，竟然还能和好几个遍历都扯上。

你看吧，**题目的解决都是从我们过去学过的知识中寻找办法**。

这道题可以用前序、中序、后序和层次遍历解决，每种遍历又可以用递归和非递归实现。

我抛砖引玉，用了前序的递归和非递归，剩下的当作作业，**还有 6 种写法**，记得不要偷懒，老老实实动手实现。

当然啦，**点赞 **也不要忘记，毕竟，要是不会写，本蛋也可以写给你们看。

我是帅蛋，我们下次见啦~

