大家好呀，今天的我还是在爬二叉树的帅蛋。

二叉树的前中后序遍历有两种实现方式：**递归和非递归**。上篇文章讲了递归法：



[**ACM 选手带你玩转二叉树前中后序遍历（递归版）**](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)



那今天就来看非递归版的二叉树前中后序遍历怎么整。

非递归的做法，很多人又叫**迭代法**。**迭代法就是不断地将旧的变量值，递推计算新的变量值**。

迭代法是怎么做呢？不卖关子，是用栈来做。

我简单给大家讲一下为什么可以用“栈”。

这个其实还是因为二叉树的前中后序遍历能用“递归”。举一个简单点的例子，下面的图是一个递归函数的运作：

<div align=center>

![qzhfdg-0](https://cdn.codegoudan.com/img/qzhfdg-0.png)

</div>

我想求 f(3)，就得知道 f(2)，那 f(2) 怎么求，当然得知道 f(1) 是啥...

你看我最先想要求的数，反而是最后才知道。

你有没有觉得这个和“我最先进去的数，反而是最后才出来”很像？这个先入后出，不就正是“栈”嘛！

没错，**递归问题的处理，可以用栈来解决**。

破案了！递归的时候竟然是隐式的维护了一个栈！没想到你竟是这样的递归！

那我们**把这个栈光明正大的模拟出来就是迭代法了**。

请喊我大聪明。

<div align=center>

![qzhfdg-1](https://cdn.codegoudan.com/img/qzhfdg-1.jpg)

</div>

如果有小婊贝还不知道栈是个什么玩意儿，麻烦先来一句“蛋蛋真帅”，再看下文：



[ACM 选手带你玩转栈和队列](https://mp.weixin.qq.com/s/4D0FQiJMJ9wA4_ejkGbwiA)



接下来，直接开整！

<div align=center>

![qzhfdg-2](https://cdn.codegoudan.com/img/qzhfdg-2.png)

</div>



我还是用**三道 LeetCode** 题来给大家具体讲解二叉树前中后序遍历的非递归版。



# 二叉树前序遍历（非递归版）



## LeetCode 144：二叉树的前序遍历

给你二叉树根节点 root，返回它节点值的前序遍历。



## 示例

<div align=center>

![qzhfdg-3](https://cdn.codegoudan.com/img/qzhfdg-3.png)

</div>

输入：root = [1,null,2,3]

输出：[1,2,3]



## 解析

前序遍历的非递归版本大概是最好写的一个了。

都知道**前序遍历的顺序是：根、左子树、右子树，也就是先输出根，再输出左子树，最后输出右子树**。

因为栈“先入后出”的特点，所以结合前序遍历的顺序，迭代的过程也就出来了：

**每次都是先将根节点放入栈，然后右子树，最后左子树。**

**具体步骤**如下所示：

- 初始化维护一个栈，将根节点入栈。

- 当栈不为空时

- - 弹出栈顶元素 node，将节点值加入结果数组中。
  - 若 node 的右子树不为空，右子树入栈。
  - 若 node 的左子树不为空，左子树入栈。



## 图解

以下图为例：

<div align=center>

![qzhfdg-4](https://cdn.codegoudan.com/img/qzhfdg-4.png)

</div>

首先初始化一个栈，将根节点入栈。

<div align=center>

![qzhfdg-5](https://cdn.codegoudan.com/img/qzhfdg-5.png)

</div>

```Python
stack = [root]
```

栈不为空，根节点出栈，将节点值加入结果数组中。

<div align=center>

![qzhfdg-6](https://cdn.codegoudan.com/img/qzhfdg-6.png)

</div>

```Python
# 根节点出栈
node = stack.pop()
# 将根节点值加入结果
res.append(node.val)
```

接下来的顺序就是，右子树入栈，左子树入栈，弹出栈顶元素。

<div align=center>

![qzhfdg-7](https://cdn.codegoudan.com/img/qzhfdg-7.png)

![qzhfdg-8](https://cdn.codegoudan.com/img/qzhfdg-8.png)

![qzhfdg-9](https://cdn.codegoudan.com/img/qzhfdg-9.png)

![qzhfdg-10](https://cdn.codegoudan.com/img/qzhfdg-10.png)

![qzhfdg-11](https://cdn.codegoudan.com/img/qzhfdg-11.png)

![qzhfdg-12](https://cdn.codegoudan.com/img/qzhfdg-12.png)

</div>

```python
while stack:
    # 根节点出栈
    node = stack.pop()
    # 将根节点值加入结果
    res.append(node.val)
    # 右子树入栈
    if node.right:
        stack.append(node.right)
    # 左子树入栈
    if node.left:
        stack.append(node.left)
```

直至栈为空，返回结果。

此解法，由于每个节点被遍历一次，所以**时间复杂度为 O(n)**。

额外维护了一个栈，**空间复杂度为 O(n)**。



## 代码实现



### Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        # 注意：根节点为空，直接返回空列表
        if not root:
            return []

        stack = [root]
        res = []

        while stack:
            # 根节点出栈
            node = stack.pop()
            # 将根节点值加入结果
            res.append(node.val)
            # 右子树入栈
            if node.right:
                stack.append(node.right)
            # 左子树入栈
            if node.left:
                stack.append(node.left)

        return res
```



### Java 代码实现

```Java
class Solution {
     public List<Integer> preorderTraversal(TreeNode root) {
      if(root == null) return res;
      Stack<TreeNode> stack = new Stack<>();
      stack.push(root);
      ArrayList<Integer> res = new ArrayList<>();
      
      while(!stack.isEmpty()) {
        TreeNode node = stack.pop();
        res.add(node.val);
        if(node.right != null) {
          stack.push(node.right);
        }
        if(node.left != null) {
          stack.push(node.left);
        }
      }
      return res;
    }
}
```



# 二叉树中序遍历（非递归版）



## LeetCode 94：二叉树的中序遍历

给你二叉树根节点 root，返回它的中序遍历。



## 示例

<div align=center>

![qzhfdg-13](https://cdn.codegoudan.com/img/qzhfdg-13.png)

</div>

输入：root = [1,null,2,3]

输出：[1,3,2]



## 解析

中序遍历的非递归就没前序遍历那么好搞了。

前序遍历是先搞根节点，再去管左子树，右子树，这就意味着，在**前序遍历中你遍历节点的顺序和你处理节点的顺序是一致的**。

这句话你要仔细品，很重要。

而中序遍历的顺序是：左子树、根、右子树，先输出左子树，再输出根节点，最后输出右子树。

这就意味着，在**中序遍历中，访问节点的顺序和处理节点的顺序是不一致的**，并且，**处理节点是在遍历完左子树之后**。

再直白点就是：**从根节点开始，一层层的遍历，找到左子树最左的那个节点，从它开始处理节点。**

例如下图中的 H 节点。

<div align=center>

![qzhfdg-14](https://cdn.codegoudan.com/img/qzhfdg-14.png)

</div>

**具体步骤**如下所示：

- 初始化一个空栈。

- 当【根节点不为空】或者【栈不为空】时，从根节点开始

- - 若当前节点有左子树，一直遍历左子树，每次将当前节点压入栈中。
  - 若当前节点无左子树，从栈中弹出该节点，尝试访问该节点的右子树。



## 图解

以下图为例：

<div align=center>

![qzhfdg-15](https://cdn.codegoudan.com/img/qzhfdg-15.png)

</div>

首先初始化一个空栈。

<div align=center>

![qzhfdg-16](https://cdn.codegoudan.com/img/qzhfdg-16.png)

</div>

```Python
stack = []
```

从根节点开始，一直向左子树遍历，同时将当前的节点压入栈中。

<div align=center>

![qzhfdg-17](https://cdn.codegoudan.com/img/qzhfdg-17.png)

![qzhfdg-18](https://cdn.codegoudan.com/img/qzhfdg-18.png)

![qzhfdg-19](https://cdn.codegoudan.com/img/qzhfdg-19.png)

</div>

```Python
# 一直向左子树走，每一次将当前节点保存到栈中
if root:
    stack.append(root)
    root = root.left
```

当前走到了最左面，弹出栈顶元素，加入结果数组中。

<div align=center>

![qzhfdg-20](https://cdn.codegoudan.com/img/qzhfdg-20.png)

</div>

```Python
if not root:
    cur = stack.pop()
    res.append(cur.val)
    root = cur.right
```

节点 2 并无右子树，继续重复上述动作。

<div align=center>

![qzhfdg-21](https://cdn.codegoudan.com/img/qzhfdg-21.png)

</div>

此时节点 0 有右子树，遍历其右子树，遍历到的节点入栈。

<div align=center>

![qzhfdg-22](https://cdn.codegoudan.com/img/qzhfdg-22.png)

</div>

接下来的动作就是在重复上面的操作。

<div align=center>

![qzhfdg-23](https://cdn.codegoudan.com/img/qzhfdg-23.png)

![qzhfdg-24](https://cdn.codegoudan.com/img/qzhfdg-24.png)

![qzhfdg-25](https://cdn.codegoudan.com/img/qzhfdg-25.png)

![qzhfdg-26](https://cdn.codegoudan.com/img/qzhfdg-26.png)



</div>

直至 root 无指向而且栈为空，返回结果。

此解法，由于每个节点被遍历一次，所以**时间复杂度为 O(n)**。

额外维护了一个栈，**空间复杂度为 O(n)**。



## 代码实现



### Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        # 注意：根节点为空，直接返回空列表
        if not root:
            return []

        stack = []
        res = []

        while root or stack:
            # 一直向左子树走，每一次将当前节点保存到栈中
            if root:
                stack.append(root)
                root = root.left
            # 当前节点为空，证明走到了最左边，从栈中弹出节点加入结果数组
            # 开始对右子树重复上述过程。
            else:
                cur = stack.pop()
                res.append(cur.val)
                root = cur.right

        return res
```



### Java 代码实现

```Java
class Solution {
  public List<Integer> inorderTraversal(TreeNode root) {
    if(root == null){
       return new int[] 
    }
    List<Integer> res = new ArrayList<Integer>();
    Stack<TreeNode> stack = new Stack<TreeNode>();
    while(stack.size() > 0 || root != null) {
      if(root != null) {
        stack.add(root);
        root = root.left;
      } 
      else {
        TreeNode cur = stack.pop();
        res.add(cur.val);
        root = cur.right;
      }
    }
    return res;
  }
}
```



# 二叉树后序遍历（非递归版）



## LeetCode 145：二叉树的后序遍历

给定一个二叉树，返回它的后序遍历。



## 示例

<div align=center>

![qzhfdg-27](https://cdn.codegoudan.com/img/qzhfdg-27.png)

</div>

输入：root = [1,null,2,3]

输出：[3,2,1]



## 解析

恭喜你，越过非递归的前序遍历和中序遍历，来到后续遍历。

**非递归版的后续遍历是三种遍历中最复杂的一个**，当然啦，只是相比而言，也不是那么复杂。

<div align=center>

![qzhfdg-28](https://cdn.codegoudan.com/img/qzhfdg-28.jpg)

</div>

我就不卖关子了，直接说怎么整。

**后序遍历的顺序是：左子树、右子树、根，先输出左子树，再输出右子树，最后输出根。**

那**对于根节点来说，它要被搞 3 次**：

- 第 1 次经过它，去向左子树。
- 第 2 次从左子树回来，经过它，去向右子树。
- 第 3 次从右子树回退到它。

<div align=center>

![qzhfdg-29](https://cdn.codegoudan.com/img/qzhfdg-29.jpg)

</div>

**具体步骤**如下所示：

- 初始化一个空栈。

- 当【根节点不为空】或者【栈不为空】时，从根节点开始

- - 每次将当前节点压入栈中，如果当前节点右左子树，就往左子树跑，没有左子树就往右子树跑。
  - 若当前节点无左子树也无右子树，从栈中弹出该节点，如果当前节点是上一个节点（即弹出该节点后的栈顶元素）的左节点，尝试访问上个节点的右子树，如果不是，那当前栈的栈顶元素继续弹出。



## 图解

以下图为例：

<div align=center>

![qzhfdg-30](https://cdn.codegoudan.com/img/qzhfdg-30.png)

</div>

首先初始化一个空栈。

<div align=center>

![qzhfdg-31](https://cdn.codegoudan.com/img/qzhfdg-31.png)

<div>

```Python
stack = []
```

从根节点开始，如果当前节点一直有左子树，那就一直向左子树遍历，同时将当前的节点压入栈中。

<div align=center>

![qzhfdg-32](https://cdn.codegoudan.com/img/qzhfdg-32.png)

![qzhfdg-33](https://cdn.codegoudan.com/img/qzhfdg-33.png)

![qzhfdg-34](https://cdn.codegoudan.com/img/qzhfdg-34.png)

</div>

```Python
while root:
    # 当前节点入栈
    stack.append(root)
    # 如果当前节点有左子树，继续向左子树找
    if root.left:
        root = root.left
```

当前走到了最左面，当前栈顶元素无左右子树，为叶子节点，弹出栈顶元素，加入结果数组中。

<div align=center>

![qzhfdg-35](https://cdn.codegoudan.com/img/qzhfdg-35.png)

</div>

```Python
# 跳出循环的条件是 root 为空，那当前栈顶元素为叶子节点。
# 弹出栈顶元素，并加入结果数组
cur = stack.pop()
res.append(cur.val)
```

当前 cur 为 当前栈顶元素的左节点，继续遍历当前栈顶元素的右子树。

<div align=center>

![qzhfdg-36](https://cdn.codegoudan.com/img/qzhfdg-36.png)

</div>

```Python
if stack and stack[-1].left == cur:
    root = stack[-1].right
```

接下来的动作就是在重复上面的操作。

<div align=center>

![qzhfdg-37](https://cdn.codegoudan.com/img/qzhfdg-37.png)

![qzhfdg-38](https://cdn.codegoudan.com/img/qzhfdg-38.png)

![qzhfdg-39](https://cdn.codegoudan.com/img/qzhfdg-39.png)

![qzhfdg-40](https://cdn.codegoudan.com/img/qzhfdg-40.png)

![qzhfdg-41](https://cdn.codegoudan.com/img/qzhfdg-41.png)

</div>

本题解同样**时间复杂度为 O(n)**，**空间复杂度为 O(n)**。



## 代码实现



### Python 代码实现

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        # 注意：根节点为空，直接返回空列表
        if not root:
            return []

        stack = []
        res = []

        while root or stack:
            while root:
                # 当前节点入栈
                stack.append(root)
                # 如果当前节点有左子树，继续向左子树找
                if root.left:
                    root = root.left
                # 如果当前节点无左子树，在右子树继续找
                else:
                    root = root.right
            # 跳出循环的条件是 root 为空，那当前栈顶元素为叶子节点。
            # 弹出栈顶元素，并加入结果数组
            cur = stack.pop()
            res.append(cur.val)
            # 如果栈不为空，且当前栈顶元素的左节点是刚刚跳出的栈顶元素 cur
            # 则转向遍历右子树当前栈顶元素的右子树
            if stack and stack[-1].left == cur:
                root = stack[-1].right
            # 否则证明当前栈顶元素无左右子树，那当前的栈顶元素弹出。
            else:
                root = None

        return res
```



### Java 代码实现

```Java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        LinkedList<TreeNode> stack = new LinkedList<>();
        while(!stack.isEmpty() || root != null){
            while(root != null){
                stack.addLast(root);
                if(root.left != null){
                    root = root.left;
                }
                else{
                    root = root.right;
                }
            }
            root = stack.removeLast();
            res.add(root.val);
            if(!stack.isEmpty() && stack.getLast().left == root){
                root = stack.getLast().right;
            }
            else{
                root = null;
            }
        }
        return res;
    }
}
```



---

哎呀妈呀，到这，**二叉树前中后续遍历（非递归版）**终于算是写完了。

自认为已经将三种遍历的非递归法讲明白了，跟着一步步的走下来，绝对没问题。

**从无到有的过程很辛苦，无所不能的样子又很酷。**

大家加油~
