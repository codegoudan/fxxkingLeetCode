设计链表，强行学明白链表的 5 种操作。

<div align=center>

![707-0](https://cdn.codegoudan.com/img/707-0.png)

</div>



# LeetCode 707：设计链表

题目链接：[设计链表](https://leetcode-cn.com/problems/design-linked-list/)



## 题意

**实现链表的查找、头插法、尾插法、通用插入、删除操作：**

- get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val 的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。



## 示例

<div align=center>

![707-1](https://cdn.codegoudan.com/img/707-1.png)

</div>



## 提示

- 1 <= val <= 1000
- 1 <= 操作次数 <= 1000
- 不能使用内置的 LinkedList 库



# 题目解析

水题，难度中等，考察链表的常规操作。

如果对链表还不太熟悉，请看下面这篇文章：

[ACM 选手带你玩转链表！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475918898&idx=1&sn=e5c7bf9c43dd205082f77beead41a187&chksm=ff22e1bfc85568a945b3e4569ea727148e6e736ccce15ec12d77c2f8ae7b8acb2eabf900e3a6&scene=21#wechat_redirect)

仔细来看，这道题主要涉及 5 种操作：

- 查找链表第 index 个节点的值
- 在链表的第一个节点前插入一个节点
- 在链表的最后一个节点后插入一个节点
- 在链表的第 index 节点前插入一个节点
- 删除链表的第 index 个节点。

这 5 种包含了链表的常见增删查操作，是刚学完链表的臭宝们及时巩固知识的绝佳练习题。

这道题我用**带头节点的单链表**来实现。

头节点，可能很多人叫做**哨兵节点**，放在第一个元素的节点之前，数据域一般没意义。



# 图解

链表题呢，为了方便后续的操作，一般上来先定义一个简单的节点类：

```Python
# 自定义单节点
class Node:
    def __init__(self, data):
        self.val = data
        self.next = None
```

链表类里初始化头节点和链表长度。

```Python
def __init__(self):
        """
        Initialize your data structure here.
        """
        # 头节点
        self.head = Node(0)
        # 保存链表长度
        self.length = 0
```

get(index) ，查找节点，没啥好说的，就是傻傻的从第 1 个节点开始找。时间复杂度 O(n)。

<div align=center>

![707-2](https://cdn.codegoudan.com/img/707-2.png)

</div>

addAtHead(val) ，在链表第一个节点前插入一个节点，很好插，找到第一个节点的前驱节点就好，在这就是头节点。

因为就在第 1 个，所以时间复杂度 O(1)。

**链表插入一定切记：插入操作的顺序不能改变！**切记！

**一定是待插入节点的后继指针先指，然后前驱节点再指向待插入节点。**

在下图对应的是值为 10 的节点先指向值为 11 的节点，然后值为 0 的头节点再指向值为 10 的节点。

<div align=center>

![707-3](https://cdn.codegoudan.com/img/707-3.png)

</div>

同理，addAtTail(val) 在链表最后一个节点后插入节点，也很简单，要插入的节点位置的前驱节点就是最后一个节点。

因为在最后一个，时间复杂度 O(n)。

<div align=center>

![707-4](https://cdn.codegoudan.com/img/707-4.png)

</div>

addAtIndex(index, val)，在链表的第 index 节点前插入一个节点，其实这个就是插入的通用操作。

同样从第一个节点开始依次查找，时间复杂度 O(n)。

addAtHead(val) 相当于 addAtIndex(0, val)。

addAtTail(val) 相当于 addAtIndex(length, val)。

<div align=center>

![707-5](https://cdn.codegoudan.com/img/707-5.png)

</div>

deleteAtIndex(index)，删除链表的第 index 个节点，同样是找到要删除节点的前驱节点，通过改变节点后继指针来删除。

同理，删除链表的时间复杂度也是 O(n)。

<div align=center>

![707-6](https://cdn.codegoudan.com/img/707-6.png)

</div>



# 代码实现

Python 代码实现

```Python
# 自定义单节点
class Node:
    def __init__(self, data):
        self.val = data
        self.next = None

class MyLinkedList:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        # 头节点
        self.head = Node(0)
        # 保存链表长度
        self.length = 0

    def get(self, index: int) -> int:
        """
        Get the value of the index-th node in the linked list. If the index is invalid, return -1.
        """
        # 判断输入的索引是否有效
        if index < 0 or index >= self.length:
            return -1

        p = self.head
        i_index = 0

        # range 是左闭右开区间，相当于[0,index+1)
        for _ in range(index + 1):
            p = p.next

        return p.val

    def addAtHead(self, val: int) -> None:
        """
        Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
        """
        return self.addAtIndex(0, val)

    def addAtTail(self, val: int) -> None:
        """
        Append a node of value val to the last element of the linked list.
        """
        return self.addAtIndex(self.length, val)

    def addAtIndex(self, index: int, val: int) -> None:
        """
        Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
        """
        # 特殊情况 index < 0 和 index = 0，则说明是头插法
        if index < 0:
            index = 0
        if index > self.length:
            return

        p = self.head
        add_node = Node(val)

        # 找到第 index 个节点的前驱节点
        for _ in range(index):
            p = p.next

        add_node.next = p.next
        p.next = add_node

        # 插入节点，链表长度 +1
        self.length += 1

    def deleteAtIndex(self, index: int) -> None:
        """
        Delete the index-th node in the linked list, if the index is valid.
        """
        # 判断输入的索引是否有效
        if index < 0 or index >=self.length:
            return

        p = self.head
        # 找到要删除节点的前驱节点和要删除的节点
        for _ in range(index + 1):
            # 前驱节点
            pre = p
            # 要删除节点
            p = p.next

        pre.next = p.next

        # 释放删除节点的内存
        p = None

        # 删除节点，链表长度 -1
        self.length -= 1

# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

