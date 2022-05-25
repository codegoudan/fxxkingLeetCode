大家好呀，我是帅蛋。

今天解决**二叉树的所有路径**，是考察二叉树的经典好题。

还是那句话，二叉树的题目，做多了你就会发现，题目之间都存在相似性，解题是有固定的套路。

闲话少说，下面让我们一起来看题吧！

<div align=center>

![257-3](https://cdn.codegoudan.com/img/257-3.png)

</div>



#  LeetCode 257：二叉树的所有路径



## 题意

给定一个二叉树的根节点 root，按任意顺序，返回所有从根节点到叶子节点的路径。



## 示例

输入：root = [1,2,3,null,5]

输出：["1->2->5", "1->3"]

<div align=center>

![257-1](https://cdn.codegoudan.com/img/257-1.png)

</div>



## 提示

叶子节点是指没有子节点的节点。

- 树中节点的数目在范围 [1, 100] 内
- -100 <= Node.val <= 100



# 题目解析

难度简单，是**考察二叉树前序遍历**的经典好题。

咦，我怎么直接把解法说出来了～

如果蛋粉们看过我之前【[**ACM 选手图解 LeetCode 二叉树的最大深度**](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)】的文章，应该会发现这题与其中的一个常规解法相似，我叫这种解法为：**自顶向下**。

类似在哪呢？就是方向上一致，维护的顺序一致：

在【[**二叉树的最大深度**](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)】中，自顶向下的解法是从根节点递归到叶子节点，计算这一条路径上的深度，并更新维护最大深度。每次先维护根节点的深度，再递归左子树、右子树。

而在本题二叉树的所有路径中，自顶向下的解法也是**从从根节点递归到叶子节点，只不过这次不是计算路径上的深度，而是记录路径上的节点，并更新维护路径。每次都是先从根节点开始，先递归左子树，再递归右子树。**

你看，这种这种每次都是先根节点，再是左子树，最后右子树，不就是前序遍历的方式嘛。

<div align=center>

![257-2](https://cdn.codegoudan.com/img/257-2.png)

</div>

# 递归法

既然是用[**递归法**](https://mp.weixin.qq.com/s/0MS7iz1qSQOZBmKY5vVxSw)，那还是按照往常，祭出递归二步曲：

**(1) 找出重复的子问题。**

前序遍历的顺序是：根节点、左子树、右子树。

在本题同样也是这个顺序：**将根节点加入路径，递归左子树，递归右子树**。

对于左子树和右子树来说，也都是同样的操作。

```Python
self.getPaths(root.left, path, res)
self.getPaths(root.right, path, res)
```

**(2) 确定终止条件。**

对于二叉树的所有路径中的每条路径，当遍历到叶子节点的时候为当前路径的结束。并且将当前路径加入结果集。

```Python
# 如果当前节点是叶子节点，将当前路径加入结果集
if root.left == None and root.right == None:
  res.append(path)
```

最重要的两点确定完了，那总的代码也就出来了。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:

    def getPaths(self, root, path, res):
        if not root:
            return
        # 节点值加入当前路径
        path += str(root.val)
        # 如果当前节点是叶子节点，将当前路径加入结果集
        if root.left == None and root.right == None:
            res.append(path)
        # 如果当前节点不是叶子节点，继续遍历左子树和右子树。
        else:
            path += '->'
            self.getPaths(root.left, path, res)
            self.getPaths(root.right, path, res)

    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        
        res = []
        self.getPaths(root, '', res)

        return res
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

    public void getPaths(TreeNode root, String path, List<String>res){
        if (root == null){
            return ;
        }
        // 节点值加入当前路径
        StringBuffer onepath = new StringBuffer(path);
        onepath.append(Integer.toString(root.val));
        // 如果当前节点是叶子节点，将当前路径加入结果集
        if (root.left == null && root.right == null){
            res.add(onepath.toString());
        }
        // 如果当前节点不是叶子节点，继续遍历左子树和右子树
        else{
            onepath.append("->");
            getPaths(root.left, onepath.toString(), res);
            getPaths(root.right, onepath.toString(), res);
        }
    }


    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<String>();
        getPaths(root, "", res);
        return res;
    }
}
```



本题解，每个节点都会被访问到，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于二叉树的高度，二叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

非递归的做法，很多人又叫**迭代法**。**迭代法就是不断地将旧的变量值，递推计算新的变量值**。

**迭代法是用栈来做**，对二叉树前序遍历迭代法不太了解的蛋粉可以看下面这篇文章：



[**ACM 选手带你玩转二叉树前中后序遍历（非递归版）**](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)



当然，**因为这道题还要记录路径，那还得需要一个额外的栈来存储路径**。

根据栈“先入后出”的特点，结合前序遍历的顺序，迭代的过程也就出来了：

**每次都是先将根节点放入栈，然后右子树，最后左子树。**

具体步骤如下所示：

- 初始化维护两个栈：一个栈是递归栈，将根节点入栈，另一个栈是路径栈，将根节点的值入栈。

- 当栈不为空时，弹出两个栈的栈顶元素：

- - 若 node 为叶子节点，将路径加入结果集。
  - 若 node 的右子树不为空，右孩子及右孩子的值分别入递归栈和路径栈。
  - 若 node 的左子树不为空，左孩子及左孩子的值分别入递归栈和路径栈。

以下图为例：

<div align=center>

![257-4](https://cdn.codegoudan.com/img/257-4.png)

</div>

首先初始化递归栈 stack、路径栈 stackPath 和结果集 res。

<div align=center>

![257-5](https://cdn.codegoudan.com/img/257-5.png)

</div>

```Python
# 初始化递归栈、路径栈和结果集
stack = [root]
stackPath = []
res = []
stackPath.append(str(root.val))
```

当栈不为空，递归栈栈顶元素 node 出栈，路径栈的栈顶元素 path 出栈。

<div align=center>

![257-6](https://cdn.codegoudan.com/img/257-6.png)

</div>

```Python
# 节点和路径出栈
node = stack.pop()
path = stackPath.pop()
```

此时 node 节点为 1，有左子树和右子树，此时先右子树入栈，再左子树入栈。

<div align=center>

![257-7](https://cdn.codegoudan.com/img/257-7.png)

</div>

```Python
# 右子树入栈
if node.right:
    stack.append(node.right)
    stackPath.append(path + "->" + str(node.right.val))
# 左子树入栈
if node.left:
    stack.append(node.left)
    stackPath.append(path + "->" + str(node.left.val))
```

接下来继续重复上面的操作。

弹出栈顶元素 node = 2，path = “1->2”，此时 node = 2 有右子树，右子树入栈。

<div align=center>

![257-8](https://cdn.codegoudan.com/img/257-8.png)

</div>

弹出栈顶元素 node = 5，path = “1->2->5”，此时 node = 5 为叶子节点，将 path 加入结果集 res。

<div align=center>

![257-9](https://cdn.codegoudan.com/img/257-9.png)

</div>

```Python
# 如果当前节点为叶子节点
if node.left == None and node.right == None:
    res.append(path)
```

弹出栈顶元素 node = 3，path = “1->3”，此时 node = 3 为叶子节点，同样将 path 加入结果集 res。

<div align=center>

![257-10](https://cdn.codegoudan.com/img/257-10.png)

</div>

当栈为空时，结束，返回此时的 res 结果集。



## Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if not root:
            return []
        # 初始化递归栈、路径栈和结果集
        stack = [root]
        stackPath = []
        res = []
        stackPath.append(str(root.val))

        while stack:
            # 节点和路径出栈
            node = stack.pop()
            path = stackPath.pop()
            # 如果当前节点为叶子节点
            if node.left == None and node.right == None:
                res.append(path)
            # 右子树入栈
            if node.right:
                stack.append(node.right)
                stackPath.append(path + "->" + str(node.right.val))
            # 左子树入栈
            if node.left:
                stack.append(node.left)
                stackPath.append(path + "->" + str(node.left.val))

        return res
```



## Java 代码实现

特别提醒：Java 可以直接定义一个成员变量为object的栈，这个栈可以放 TreeNode，可以放 String，这样就不需要搞两个栈，都放在一个栈里即可。

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
    public List<String> binaryTreePaths(TreeNode root) {
        // 初始化
        List<String> res = new ArrayList<>();
        if (root == null)
            return res;

        Stack<Object> stack = new Stack<>();
        stack.push(root);
        stack.push(root.val + "");

        while (!stack.isEmpty()) {
            // 节点和路径出栈
            String path = (String) stack.pop();
            TreeNode node = (TreeNode) stack.pop();
            // 如果当前节点为叶子节点
            if (node.left == null && node.right == null) {
                res.add(path);
            }
            // 右子树入栈
            if (node.right != null) {
                stack.push(node.right);
                stack.push(path + "->" + node.right.val);
            }
            // 左子树入栈
            if (node.left != null) {
                stack.push(node.left);
                stack.push(path + "->" + node.left.val);
            }
        }
        return res;
    }
}
```

同样，本题解**时间复杂度为 O(n)**，**空间复杂度为 O(n)**。

---

**求解二叉树的所有路径**到这就结束辣，是不是觉得自己的功力更上一层楼了？

求个路径也能和前序遍历扯上关系，你看吧，**题目的解决都是从我们过去学过的知识中寻找办法**。

教给你这么有意思的二叉树，不得赶紧给我来个**点赞 **？

哈哈哈哈，必须得有是吧~

我是帅蛋，我们下次见！
