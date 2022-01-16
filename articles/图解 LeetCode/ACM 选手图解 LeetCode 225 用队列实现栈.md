**用队列实现栈**这道题，和用栈实现队列一样是考察对”栈和队列理解程度“的好题。

但是用队列实现栈，和用栈实现队列是两种不同的移动思路。

这种题都是为了考察而考察，没啥工程意义，实际工作中要是有人敢让你这么做，不要说话，先给他一 jue。

话不多说，直接开整。

<div align=center>

![19b5b4f04c1749c80129a3a5efc11e6](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_184119700_0.jpg)

</div>

# LeetCode 225：用队列实现栈

题目链接：[用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)



## 题意

**用队列实现栈的下列操作：**

- void push(int x)：元素 x 入栈。
- int pop()：移除并返回栈顶元素。
- int peek()：返回栈顶元素。
- boolean empty()：栈为空，返回 true，否则返回 false。



## 示例

<div align=center>

![d574f58cba262d0590b1c8f37a46d08](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_184422011_0.jpg)

</div>



## 提示

- 1 <= x <= 9
- 最多调用 100 次 push、pop、top 和 empty
- 每次调用 pop 和 top 都保证栈不为空



# 题目解析

水题，难度简单，**主要还是考察对栈和队列的理解能力**。

如果对栈和队列还不熟悉，看一下本帅比写的这篇文章：



[呔！“栈”住，队列！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475919772&idx=1&sn=8574188f72d892d501f15956b6a2a629&chksm=ff22ee11c8556707dd6d5af977125ce6698ebbcfaa3b9083e14c3658d2975761079a9c2082a4&scene=21#wechat_redirect)

和栈实现队列相仿，用队列实现栈也涉及 4 步操作：

- 入栈 push
- 出栈 pop
- 取栈顶元素 peek
- 判空 empty

知道了需求，下面就是如何用队列模拟栈。

这里有**两种**方法：

1. 两个队列模拟。
2. 一个队列模拟。

**两个队列模拟**

建一个主队列和一个辅助队列，每次入栈操作时，将新元素添加到辅助队列，再依次将主队列的元素出队列，依次加入辅助队列，最后将主队列与辅助队列互换。

**一个队列模拟**

建一个主队列，其余操作正常，只是在每次模拟栈弹出元素的时候，将除了最后一个元素外的之前元素弹出队列，重新添加到主队列尾部即可。



# 图解

为了照顾大多数臭宝，我用**两个队列**来模拟，笨笨的方法更直观一些。

首先初始化主队列和辅助队列。

<div align=center>

![a873becbcc63431031412d6e4c68bfd](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_184545176_0.jpg)

</div>

```Python
def __init__(self):
    self.major_queue = []
    self.help_queue = []
```

push(x)，入栈操作，先压入辅助队列，再将主队列的元素加入辅助队列，再互换主队列和辅助队列。

首先 push(1)：

<div align=center>

![3b78f51d94f412eebcc1560143342e6](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_184618600_0.jpg)

</div>

再 push(2)：

<div align=center>

![16a210202815ea164a50dd77c5f0587](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_184635333_0.jpg)

</div>

```Python
def push(self, x: int) -> None:
    # 新元素放入辅助队列的队首
    self.help_queue.append(x)
    # 将主队列的元素加入辅助队列
    while self.major_queue:
        self.help_queue.append(self.major_queue.pop(0))
    # 现在辅助队列元素是和栈的元素排列一致，为了后续方便理解，交换两个队列的内容
    self.major_queue = self.help_queue
    self.help_queue = []
```

入栈需要将所有的元素都倒腾一遍，所以入栈的时间复杂度为 O(n)，因为需要额外的队列存储栈内的元素，所以入栈的空间复杂度为 O(n)。

pop()，出栈操作，出栈出的是栈顶元素，由上图可以看出，栈顶元素即主队列的队首元素。

<div align=center>

![7afaff00fc3ccc5ec2bdf4e230f5ef8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_184712386_0.jpg)

</div>

```Python
def pop(self) -> int:
    # 判断是否为空
    if self.empty():
        return None
    # pop移除栈顶元素，相当于移除主队列的队首元素
    return self.major_queue.pop(0)
```

出栈对应的是将主队列的队首元素出栈，所以时间复杂度是 O(1)，空间复杂度为 O(n)。

top()，获取栈顶元素，与 pop 类似，即相当于获取主队列的队首元素。

```Python
def top(self) -> int:
    # top获取栈顶元素，即获取主队列的队首元素
    return self.major_queue[0]
```

同样，获取栈顶元素是获取主队列的首元素，所以时间复杂度为 O(1)，空间复杂度为 O(n)。

empty()，判断栈是否为空。其实就是看主队列是否为空，主队列为空，栈就空，主队列不为空，栈不为空。

```Python
def empty(self) -> bool:
    # 只需要判断主队列是否为空
    if not self.major_queue:
        return True
    return False
```

对于判空，只需要判断主队列是否为空，所以时间复杂度为 O(1)，空间复杂度为 O(n)。



# 代码实现



## Python 代码实现

```Python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        # 初始化主队列和辅助队列
        self.major_queue = []
        self.help_queue = []

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        # 新元素放入辅助队列的队首
        self.help_queue.append(x)
        # 将主队列的元素加入辅助队列
        while self.major_queue:
            self.help_queue.append(self.major_queue.pop(0))

        # 现在辅助队列元素是和栈的元素排列一致，为了后续方便理解，交换两个队列的内容
        self.major_queue = self.help_queue
        self.help_queue = []


    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        # 判断是否为空
        if self.empty():
            return None
        # pop移除栈顶元素，相当于移除主队列的队首元素
        return self.major_queue.pop(0)


    def top(self) -> int:
        """
        Get the top element.
        """
        # top获取栈顶元素，即获取主队列的队首元素
        return self.major_queue[0]

    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        # 只需要判断主队列是否为空
        if not self.major_queue:
            return True
        return False
        
# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```



## Java 代码实现

```Java
class MyStack {

    Queue<Integer> queue1; // 和栈中保持一样元素的队列
    Queue<Integer> queue2; // 辅助队列

    /** Initialize your data structure here. */
    public MyStack() {
        queue1 = new LinkedList<>();
        queue2 = new LinkedList<>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue2.offer(x); // 先放在辅助队列中
        while (!queue1.isEmpty()){
            queue2.offer(queue1.poll());
        }
        Queue<Integer> queueTemp;
        queueTemp = queue1;
        queue1 = queue2;
        queue2 = queueTemp; // 最后交换queue1和queue2，将元素都放到queue1中
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue1.poll(); // 因为queue1中的元素和栈中的保持一致，所以这个和下面两个的操作只看queue1即可
    }
    
    /** Get the top element. */
    public int top() {
        return queue1.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue1.isEmpty();
    }
}
```



