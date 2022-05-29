大家好，我是深水炸蛋。

上篇文章解决了【**[二叉树的最大深度](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)**】，那今天就来多解决几个叉，搞一下 **N 叉树的最大深度**。

如果你仔细看过我在[**二叉树的最大深度**](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)中的讲解，这道题对你来说没有难度。

为啥这么说呢？因为这本质上就是一类题。N 叉本就包含着二叉，解题的套路都是一样的。

那为啥还要再搞下这道题？我就是想看看有没有人没学废，抓出来打屁股。

<div align=center>

![559-0](https://cdn.codegoudan.com/img/559-0.png)

</div>



# LeetCode 559：N 叉树的最大深度

题目链接：[N 叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)



## 题意

给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。



## 示例

输入：root = [1,null,3,2,4,null,5,6]
输出：3

<div align=center>

![559-1](https://cdn.codegoudan.com/img/559-1.png)

</div>



## 提示

- 树的深度不超过 1000。
- 树的节点数目位于 [0, 10^4] 之间。



# 题目解析

难度简单，求树的深度的通用题目。

还是那句话，你想了解这道题的做法，你得先搞明白 3 个概念：深度、高度、层次。

**二叉树的深度是从根节点开始算起，依次往下是深度 1、2、...。可以理解成一口井，从上往下看，也就是自顶向下看**。

**二叉树的高度是从叶子节点算起，叶子节点高度是 1，依次往上类推**。**可以看成是高楼，从下往上看，也就是自底向上看**。

**二叉树的层次是从根节点算起，根节点是第一层，依次往下类推。**

此外深度、高度、层次之间有如下关系：

**二叉树的深度和层次是完全对应的，最大深度为最大层次数。二叉树的深度和高度正好相反**。

根据这 3 个概念，对应出解决 N 叉树最大深度的 3 个解题方法：

**(1) 自顶向下**

自顶向下，就是从根节点递归到叶子节点，计算这一条路径上的深度，并更新维护最大深度。

这个是正儿八经的求深度。**每次先维护根节点的深度，再递归所有的子树**。

<div align=center>

![559-2](https://cdn.codegoudan.com/img/559-2.png)

</div>

**(2) 自底向上**

自底向上，从叶子节点开始，一层一层的向上，最终汇集在根节点。

**这种求的其实是 N 叉树的高度**。递归的遍历各个子树的最大高度，比出最大的高度 maxChildHeight，加上根节点的高度 1，即为当前 N 叉树的最大高度。

因为 **N 叉树的最大高度 = 最大深度**，所以即可求出 N 叉树的最大深度。

<div align=center>

![559-3](https://cdn.codegoudan.com/img/559-3.png)

</div>



**(3) 自左向右**

自左向右，就是从根节点开始，一层一层的遍历 N 叉树。

**这种求的是 N 叉树的层次。N 叉树的最大层次就是其最大深度。**

可以看出，这种一层一层的遍历，其实用的就是**层次遍历**。

<div align=center>

![559-4](https://cdn.codegoudan.com/img/559-4.png)

</div>



# 递归法

递归法，我以自底向上方式为例，解决本题，相比而言，对于递归法来说，这个方式更好理解。

用[**递归法**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475925238&idx=1&sn=ea7eafee3e61b433642312f33a5a2413&chksm=ff22f97bc855706d385c42fbb5132da5c7a2a53a2ca59e6a475f7ccfb3c2dc69195f6716e58a&scene=21#wechat_redirect)，按照套路我们要祭出递归二步曲：

**(1) 找出重复的子问题。**

这个问题很简单，找出子树的最大高度。同样对于各个子树来说，也都是同样的操作。

```Python
# 递归计算各个子树的高度
for child in root.children:
    maxChildHeight = max(maxChildHeight, self.maxDepth(child))
```



其实就是下面这张图的样子：

<div align=center>

![559-5](https://cdn.codegoudan.com/img/559-5.png)

</div>

**(2) 确定终止条件。**

对于 N 叉树来说，想终止，即没东西遍历了，没东西遍历自然就停下来了。

那就是当前节点是空的，既然是空的那就没啥好遍历。

```Python
# 节点为空，高度为 0
if root == None:
    return 0
```



## Python 代码实现

```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        # 节点为空，高度为 0
        if root == None:
            return 0
        # 记录子树的最大高度
        maxChildHeight = 0
        # 递归计算各个子树的高度
        for child in root.children:
            maxChildHeight = max(maxChildHeight, self.maxDepth(child))
        # N 叉树的最大高度 = 子树的最大高度 + 1（1 是根节点）
        return maxChildHeight + 1
```



## Java 代码实现

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
    public int maxDepth(Node root) {
        // 节点为空，高度为 0
        if(root == null){
            return 0;
        }
        // 记录子树的最大高度
        int maxChildHeight = 0;
        // 递归计算各个子树的高度
        for(Node child : root.children){
            maxChildHeight = Math.max(maxChildHeight, maxDepth(child));
        }
        // N 叉树的最大高度 = 子树的最大高度 + 1（1 是根节点）
        return maxChildHeight + 1;
    }
}

```



本题解，在递归过程中每个节点都被遍历到，**时间复杂度为 O(n)**。

此外在递归过程中调用了额外的栈空间，栈的大小取决于 N 叉树的高度，N 叉树最坏情况下的高度为 n，所以**空间复杂度为 O(n)**。



# 非递归法（迭代）

迭代法，我以自左向右，即层次遍历的方式为例。

我之前详细的写过层次遍历，不清楚层次遍历的小宝贝可以去看下：



[ACM 选手带你玩转二叉树层次遍历（递归 + 非递归）](https://mp.weixin.qq.com/s/3MMgFtLW9BHpguUkHICLkQ)



我在文章中讲过，**非递归版的层次遍历用的则是队列**，这是由于层次遍历的属性非常契合队列的特点。

具体做法就是：**使用队列保存每一层的所有节点，把队列里的所有节点出队列，然后把这些出去节点各自的子节点入队列**。用 depth 维护每一层。

比如对于下图：

<div align=center>

![559-6](https://cdn.codegoudan.com/img/559-6.png)

</div>

首先初始化队列 queue 和层次 depth，将根节点入队列：

<div align=center>

![559-7](https://cdn.codegoudan.com/img/559-7.png)

</div>

```Python
# 初始化队列和层次
queue = [root]
depth = 0
```



当队列不为空，出队列，将所有的子节点入队列。

<div align=center>

![559-8](https://cdn.codegoudan.com/img/559-8.png)

</div>

```Python
# 当队列不为空
while queue:
    # 当前层的节点数
    n = len(queue)
    # 弹出当前层的所有节点，并将所有子节点入队列
    for i in range(n):
        node = queue.pop(0)
        for child in node.children:
            queue.append(child)

    depth += 1
```



下面就是按照上面的方式，出队列，入队列，维护当前层次，直至队列为空。

<div align=center>

![559-9](https://cdn.codegoudan.com/img/559-9.png)

</div>



## Python 代码实现

```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        # 空树，高度为 0
        if root == None:
            return 0
        # 初始化队列和层次
        queue = [root]
        depth = 0
        # 当队列不为空
        while queue:
            # 当前层的节点数
            n = len(queue)
            # 弹出当前层的所有节点，并将所有子节点入队列
            for i in range(n):
                node = queue.pop(0)
                for child in node.children:
                    queue.append(child)

            depth += 1
        # N 叉树最大层次即为 N 叉树最大深度
        return depth
```



## Java 代码实现

```Java
class Solution {
    public int maxDepth(Node root) {
        // 节点为空，高度为 0
        if(root == null){
            return 0;
        }

        // 初始化队列和层次
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;

        // 当队列不为空
        while(!queue.isEmpty()){
            // 当前层的节点数
            int n = queue.size();
            // 弹出当前层的所有节点，并将所有子节点入队列
            for(int i = 0; i < n; i++){
                Node node = queue.poll();
                for(Node child : node.children){
                    queue.offer(child);
                }
            }
            depth++;
        }
        // N 叉树最大层次即为 N 叉树最深深度
        return depth;
    }
}
```



本题解，对于每个节点，各进出队列一次，所以**时间复杂度为 O(n)**。

此外，额外维护了一个队列，所以**空间复杂度为 O(n)**。



---

**图解 N 叉树的最大深度**到这就结束辣，没骗你们吧？就是和在解决[**二叉树的最大深度**](https://mp.weixin.qq.com/s/4n_L-px-5D2mUkmC62H5Dw)用的是一个套路。

N 叉树的最大深度可以看成是一个求树的最大深度的标准模板，毕竟这个搞会了，以后什么二叉树三叉树四叉树...都就会了。

多做题多思考多积累，慢慢自己”蛋“药库的武器就会越来越多，最终由量变到质变。

如果认可，记得帮我**点赞**，让我看到你！

我是帅蛋，我们下次见啦！
