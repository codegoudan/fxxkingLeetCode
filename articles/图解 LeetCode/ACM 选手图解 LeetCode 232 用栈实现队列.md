**用栈实现队列**这道题，是考察对”栈和队列理解程度“的好题。

放心，在实际工作的时候，不是脑残十级，几乎不会提出这样奇怪的需求。

话不多说，直接开整。

<div align=center>

![232-0](https://cdn.codegoudan.com/img/232-0.png)

</div>

# LeetCode 232：用栈实现队列

题目链接：[用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)



## 题意

**仅使用两个栈实现先入先出队列，队列支持一般队列支持的所有操作：**

- void push(int x)：将元素 x 推到队列的末尾。
- int pop()：从队列的开头移除并返回元素。
- int peek()：返回队列开头的元素。
- boolean empty()：如果队列为空，返回 true；否则，返回 false。



## 示例

<div align=center>

![232-1](https://cdn.codegoudan.com/img/232-1.png)

</div>

## 提示

- 1 <= x <= 9
- 最多调用 100 次 push、pop、peek 和 empty
- 假设所有操作都是有效的（eg. 一个空的队列不会调用 pop 或者 peek 操作）



# 题目解析

水题，难度简单，**主要考察对栈和队列的理解能力**。

如果对栈和队列还不熟悉，看一下下面这篇文章，某帅比写的：



[呔！“栈”住，队列！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475919772&idx=1&sn=8574188f72d892d501f15956b6a2a629&chksm=ff22ee11c8556707dd6d5af977125ce6698ebbcfaa3b9083e14c3658d2975761079a9c2082a4&scene=21#wechat_redirect)



仔细来看，主要涉及 4 种常规操作：

- 入队 push
- 出队 pop
- 判空 empty
- 取队首元素 peek

知道了要求，剩下就是如何用栈模拟队列。

队列是一种先入先出（FIFO）的数据结构，而栈是一种后入先出（LIFO）的数据结构，所以一个栈绝对满足不了队列的 FIFO 的特性。

比如 1 2 3，队列 1 2 3 进，应该 1 2 3 出，但是 1 2 3 进了栈，出来以后会成 3 2 1，和 1 2 3 是相反的，所以再需要一个栈，把 3 2 1 返成 1 2 3。

因此这里需要两个栈，分别是**输入栈和输出栈**：

**输入栈来反转元素的入队顺序，元素入只能从输入栈进（push）。**

**输出栈用来存储元素的正常顺序，元素出只能从输出栈出（pop、peek）。**



# 图解

首先初始化输入栈和输出栈。

<div align=center>

![232-2](https://cdn.codegoudan.com/img/232-2.png)

</div>

```Python
def __init__(self):
    # 初始化输入栈和输出栈
    self.inStack = []
    self.outStack = []
```

push(x) ，入队操作，直接压入输入栈即可。

比如 push(1)、push(2)。

<div align=center>

![232-3](https://cdn.codegoudan.com/img/232-3.png)

</div>

```Python
def push(self, x: int) -> None:
    # 有新元素进来，进入输入栈
    self.inStack.append(x)
```

push 操作，每个元素入栈的时间复杂度为 O(1)，队列长度为 n，所以时间复杂度为 O(n)。因为需要额外的栈来存储队列中的元素，所以空间复杂度为 O(n)。

pop()， 出队操作，如果输出栈不为空的话，直接扔出栈顶元素，输出栈为空的话，那把输入栈的所有元素压入输出栈中，然后再扔出栈顶元素。

<div align=center>

![232-4](https://cdn.codegoudan.com/img/232-4.png)

</div>

```Python
def pop(self) -> int:
    # 如果为空
    if self.empty():
        return None
    # 如果输出栈不为空，返回输出栈中的元素
    if self.outStack:
        return self.outStack.pop()
    # 输出栈为空,将输入栈的元素压入输出栈
    else:
        while self.inStack:
            val = self.inStack.pop()
            self.outStack.append(val)
        return self.outStack.pop()
```

pop 操作的时间复杂度为 O(n)，空间复杂度为 O(n)。

empty()，判空操作。判空很简单，输入栈和输出栈都为空，则队列为空，否则队列不为空。

<div align=center>

![232-5](https://cdn.codegoudan.com/img/232-5.png)

</div>

```Python
def empty(self) -> bool:
      # 两个栈都为空，队列才为空
      if not(self.inStack or self.outStack):
          return True

      return False
```

这个没啥好说的，做了一步判断，时间复杂度为 O(1)，空间复杂度均为 O(n)。

peek()，取队列的首元素，和 pop() 的操作差不多。唯一的区别是，pop 是删掉了队首元素，而 peek 只是看队首元素。

```Python
def peek(self) -> int:
    # 使用已有的函数 pop
    res = self.pop()
    # pop 函数弹出了 res，所以要再添加回去
    self.outStack.append(res)
  
    return res
```

队列的首元素使用已有的 pop 函数，时间复杂度和空间复杂度和 pop 一样，**时间复杂度和空间复杂度都为 O(n)**。



# 代码实现



## Python 代码实现

```Python
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        # 初始化输入栈和输出栈
        self.inStack = []
        self.outStack = []

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        # 有新元素进来，进入输入栈
        self.inStack.append(x)

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        # 如果为空
        if self.empty():
            return None

        # 如果输出栈不为空，返回输出栈中的元素
        if self.outStack:
            return self.outStack.pop()
        # 输出栈为空,将输入栈的元素压入输出栈
        else:
            while self.inStack:
                val = self.inStack.pop()
                self.outStack.append(val)
            return self.outStack.pop()

    def peek(self) -> int:
        """
        Get the front element.
        """
        # 使用已有的函数 pop
        res = self.pop()
        # pop 函数弹出了 res，所以要再添加回去
        self.outStack.append(res)

        return res


    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        # 两个栈都为空，队列才为空
        if not(self.inStack or self.outStack):
            return True

        return False



# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```



## Java 代码实现

```Java
class MyQueue {

    Stack<Integer> stack1;
    Stack<Integer> stack2;

    /** Initialize your data structure here. */
    public MyQueue() {
        stack1 = new Stack<>(); // 负责进栈
        stack2 = new Stack<>(); // 负责出栈
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        stack1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {    
        dumpStack1();
        return stack2.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        dumpStack1();
        return stack2.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack1.isEmpty() && stack2.isEmpty();
    }

    // 如果stack2为空，那么将stack1中的元素全部放到stack2中
    private void dumpStack1(){
        if (stack2.isEmpty()){
            while (!stack1.isEmpty()){
                stack2.push(stack1.pop());
            }
        }
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```



---

**图解栈实现队列**到这就结束啦，是不是很简单呐，只要熟悉栈和队列就一定会。

