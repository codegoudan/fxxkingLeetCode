大家好呀，我是搜索蛋。

今天解决**二叉搜索树中的众数**，众数是一个数学名词，常用在统计学中，是一组数据中出现次数最多的数值，代表数据的一般水平。

知道了这个，下面让我们一起来看这道题！

<div align=center>

![501-0](https://cdn.codegoudan.com/img/501-0.png)

</div>



# LeetCode 501：二叉搜索树中的众数

题目链接：[二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)



## 题意

给你一个含重复值的二叉搜索树的根节点 root，找出并返回其中的所有众数，即出现频率最高的元素。

如果树中有不止一个众数，可以按任意顺序返回。



## 示例

输入：root = [1,null,2,2]
输出：[2]

<div align=center>

![501-1](https://cdn.codegoudan.com/img/501-1.png)

</div>

## 提示

- 树中节点的数目在范围 [1,10^4] 内
- -10^5 <= Node.val <= 10^5



# 题目解析

这道二叉搜索树中的众数，难度简单，题目经典。

解决这道题依然还是用到了二叉搜索树的性质：**二叉搜索树进行中序遍历时，得到的结果是一个有序的序列。**

如果不懂二叉搜索树，可以看我下面这篇文章：



**[ACM 选手带你玩转二叉搜索树！](https://mp.weixin.qq.com/s/DsWb4oXeOWsCRiuPRwJysg)**



中序遍历以后，这道题就变成了：**在有序序列中寻找众数。**

可能看到“寻找众数”，很多同学脑子里会直接浮现出“哈希表”，但是我劝你还是别，太浪费空间了。

这里还是利用“有序序列”的这个特性，**因为有序，所有重复的数字一定是连续的，即集中在某个数据段内，这样我们就一遍扫描，判断相邻的元素值是否相等即可。**

具体的解法我们往下看~

<div align=center>

![501-8](https://cdn.codegoudan.com/img/501-8.jpg)

</div>



# 递归法

根据上面讲的，递归法其实就是二叉树中序遍历的递归法。



**[ACM 选手带你玩转二叉树前中后序遍历（递归版）](https://mp.weixin.qq.com/s/8O7cIqyU6Ecpcq3Nj_e_0Q)**



但凡是递归我们就分为两步来看：



**(1) 找出重复的子问题。**

中序遍历的顺序是：左子树、根、右子树。

对于左子树、右子树来说，也是同样的遍历顺序。

所以这个重复的子问题就是：**先遍历左子树、再取根节点，最后遍历右子树**。

```Python
self.inOrder(root.left, lst)
lst.append(root.val)
self.inOrder(root.right, lst)
```



**(2) 确定终止条件。**

和前序遍历相同，就是当前的节点为空，空的没啥好遍历的。

```Python
if root == None:
    return None
```



最重要的两点确定完了，然后代码基本上就出来。

**最后记得在主函数中求众数，也就是在有序序列中求众数。**

因为**可能存在多个众数**，我们用 cnt 记录当前元素重复的次数，用 maxCnt 记录已经遍历过的元素当中出现最多的元素的出现次数，用 res 记录众数。

具体做法就是：

- 比较当前的元素值 lst[i] 与前一个元素值 pre：

- - 如果 lst[i] == pre，cnt = cnt + 1。
  - 如果 lst[i] > pre，cnt = 1。

- 比较当前元素重复的次数 cnt 与 maxCnt：

- - 如果 cnt == maxCnt，说明当前的元素值 lst[i] 出现的次数等于当前众数出现的次数，将 lst[i] 加入 res。
  - 如果 cnt > maxCnt，说明当前的元素值 lst[i] 出现的次数大于当前众数出现的次数，所以将 maxCnt 更新为 cnt，清空 res，之后将 lst[i] 加入 res。

<div align=center>

![501-9](https://cdn.codegoudan.com/img/501-9.jpg)

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
    # 中序遍历
    def inOrder(self, root: TreeNode, lst):
        if root == None:
            return None
        self.inOrder(root.left, lst)
        lst.append(root.val)
        self.inOrder(root.right, lst)

    def findMode(self, root: TreeNode) -> List[int]:
        lst = []
        self.inOrder(root, lst)
        # 记录前一个元素值
        pre = lst[0]
        # 记录次数
        cnt = 1
        # 记录最大次数
        maxCnt = 1
        # 记录结果
        res = [lst[0]]
        
        for i in range(1, len(lst)):
            # 如果与前一个节点的值相等
            if pre == lst[i]:
                cnt += 1
            else:
                cnt = 1
            # 如果和最大次数相同，将值放入 res
            if cnt == maxCnt:
                res.append(lst[i])
            # 如果大于最大次数
            if cnt > maxCnt:
                # 更新最大次数
                maxCnt = cnt
                # 重新更新 res
                res = [lst[i]]
            pre = lst[i]
        
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

    // 中序遍历
    public void inOrder(TreeNode root, List<Integer> lst) {
        if (root == null) {
            return;
        }
        inOrder(root.left, lst);
        lst.add(root.val);
        inOrder(root.right, lst);
    }

    public int[] findMode(TreeNode root) {
        List<Integer> lst = new ArrayList<Integer>();
        inOrder(root, lst);
        // 记录前一个元素值
        int pre = lst.get(0);
        // 记录次数
        int cnt = 1;
        // 记录最大次数
        int maxCnt = 1;
        // 记录结果
        List<Integer> res = new ArrayList<Integer>();
        res.add(lst.get(0));
        for(int i = 1; i < lst.size(); i++){
            // 如果与前一个节点的值相等
            if(pre == lst.get(i)){
                cnt += 1;
            }
            else{
                cnt = 1;
            }
            // 如果和最大次数相同，将值放入 res
            if (cnt == maxCnt){
                res.add(lst.get(i));
            }
            // 如果大于最大次数
            else if(cnt > maxCnt){
                // 更新最大次数
                maxCnt = cnt;
                // 重新更新 res
                res.clear();
                res.add(lst.get(i));
            }
            pre = lst.get(i);
        }
        return res.stream().mapToInt(Integer::intValue).toArray();
    }
}
```



此解法，中序遍历每个节点被遍历一次，所以**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，维护了一个 res 的结果数组，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

对于中序遍历而言，**处理节点是在遍历完左子树之后**。

直白点就是：**从根节点开始，一层层的遍历，找到左子树最左的那个节点，从它开始处理节点。**

如果对非递归的二叉树中序遍历不熟悉的同学，可以看下面这篇文章：



**[ACM 选手带你玩转二叉树前中后序遍历（非递归版）](https://mp.weixin.qq.com/s/5m0AACLIIve26cFbjjsCVQ)**



常规的**中序遍历具体步骤**如下所示：

- 初始化一个空栈。

- 当【根节点不为空】或者【栈不为空】时，从根节点开始

- - 若当前节点有左子树，一直遍历左子树，每次将当前节点压入栈中。
  - 若当前节点无左子树，从栈中弹出该节点，尝试访问该节点的右子树。

在我这道题中，我们还需要求【有序序列的众数】，众数是一组数据中出现次数最多的数值。

其实和在递归中我介绍的求众数的方法类似，只不过在递归法中，我是中序遍历完，再求的众数，而在非递归法中，我们是一边弹出节点一边维护众数。

<div align=center>

![501-10](https://cdn.codegoudan.com/img/501-10.jpg)

</div>



具体做法就是：

- 比较当前弹出的节点 cur 与前一个节点 pre：

- - 如果 cur.val == pre.val，cnt = cnt + 1。
  - 如果 cur.val > pre.val，cnt = 1。

- 比较当前元素重复的次数 cnt 与 maxCnt：

- - 如果 cnt == maxCnt，说明当前的元素值 cur.val 出现的次数等于当前众数出现的次数，将 cur.val 加入 res。
  - 如果 cnt > maxCnt，说明当前的元素值 cur.val 出现的次数大于当前众数出现的次数，所以将 maxCnt 更新为 cnt，清空 res，之后将 cur.val 加入 res。

<div align=center>

![501-11](https://cdn.codegoudan.com/img/501-11.jpg)

</div>

以下图为例：

<div align=center>

![501-2](https://cdn.codegoudan.com/img/501-2.png)

</div>

首先初始化一个空栈 stack、记录前一个元素值 pre、记录当前元素重复的次数 cnt，记录已经遍历过的元素当中出现最多的元素的出现次数 maxCnt，记录结果 res。

<div align=center>

![501-3](https://cdn.codegoudan.com/img/501-3.png)

</div>

```Python
stack = []
 # 记录前一个元素值
pre = None
 # 记录次数
cnt = 0
# 记录最大次数
maxCnt = 0
 # 记录结果
res = []
```



第 1 步，从根节点开始，一直向左子树遍历，同时将当前的节点压入栈中。

<div align=center>

![501-4](https://cdn.codegoudan.com/img/501-4.png)

</div>

```Python
# 一直向左子树走，每一次将当前节点保存到栈中
if root:
    stack.append(root)
    root = root.left
```



当前走到了最左面，弹出栈顶元素，即 cur 为节点 1。

因为 pre == None，证明刚遍历到第 1 个节点，将 cnt 赋值为 1。

<div align=center>

![501-5](https://cdn.codegoudan.com/img/501-5.png)

</div>

```Python
cur = stack.pop()
# 第一个节点
if pre == None:
    cnt = 1
```



此时的 cnt > maxCnt，所以令 maxCnt = cnt，将 res 清空，之后将 cur.val 加入 res。

<div align=center>

![501-6](https://cdn.codegoudan.com/img/501-6.png)

</div>

```Python
# 如果大于最大次数
if cnt > maxCnt:
    # 更新最大次数
    maxCnt = cnt
    # 重新更新 res
    res = [cur.val]
```



下面将 pre 指向 cur，同时开始遍历右子树。

<div align=center>

![501-7](https://cdn.codegoudan.com/img/501-7.png)

</div>

之后的每一步都是不断的入栈、出栈，维护 pre、cnt、maxCnt 以及 res，直至遍历完整棵二叉搜索树。

我就不在此过多赘述，大家自己动手试一下。

<div align=center>

![501-12](https://cdn.codegoudan.com/img/501-12.jpg)

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
    def findMode(self, root: TreeNode) -> List[int]:
        stack = []
         # 记录前一个元素值
        pre = None
         # 记录次数
        cnt = 0
        # 记录最大次数
        maxCnt = 0
         # 记录结果
        res = []

        while root or stack:
            # 一直向左子树走，每一次将当前节点保存到栈中
            if root:
                stack.append(root)
                root = root.left
            # 当前节点为空，证明走到了最左边，从栈中弹出节点
            # 开始对右子树重复上述过程
            else:
                cur = stack.pop()
                # 第一个节点
                if pre == None:
                    cnt = 1
                # 如果与前一个节点的值相等
                elif pre.val == cur.val:
                    cnt += 1
                else:
                    cnt = 1
                # 如果和最大次数相同，将值放入 res
                if cnt == maxCnt:
                    res.append(cur.val)
                 # 如果大于最大次数
                if cnt > maxCnt:
                    # 更新最大次数
                    maxCnt = cnt
                    # 重新更新 res
                    res = [cur.val]
                pre = cur
                root = cur.right
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
    public int[] findMode(TreeNode root) {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        // 记录前一个节点
        TreeNode pre = null;
        // 记录次数
        int cnt = 0;
        // 记录最大次数
        int maxCnt = 0;
        // 记录结果
        List<Integer> res = new ArrayList<Integer>();

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
                // 第一个节点
                if(pre == null){
                    cnt = 1;
                }
                // 如果与前一个节点的值相等
                else if(pre.val == cur.val){
                    cnt += 1;
                }
                else{
                    cnt = 1;
                }
                // 如果和最大次数相同，将值放入 res
                if(cnt == maxCnt){
                    res.add(cur.val);
                }
                // 如果大于最大次数
                else if(cnt > maxCnt){
                    // 更新最大次数
                    maxCnt = cnt;
                    // 重新更新 res
                    res.clear();
                    res.add(cur.val);
                }
                pre = cur;
                root = cur.right;
            }
        }
        return res.stream().mapToInt(Integer::intValue).toArray();
    }
}
```



同样，非递归版的解法**时间复杂度为** **O(n)，空间复杂度为 O(n)**。



---

**图解二叉搜索树中的众数**到这就结束辣，在搞定二叉搜索树上我们又走远了一步。

问题就是这么一步步的抽丝剥茧，将难题拆成一步步的简单题，直到解决。

下面的时间我还会继续带大家来玩转二叉搜索树，敬请期待。

<div align=center>

![501-13](https://cdn.codegoudan.com/img/501-13.jpg)

</div>

我是帅蛋，我们下次见！
