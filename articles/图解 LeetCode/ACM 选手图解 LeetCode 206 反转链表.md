**反转链表**，它没有太多思维上的难度，但是出现频率极高。

可以这么说，不管什么时候，只要考到链表，反转链表都属于必考项。

<div align=center>

![206-0](https://cdn.codegoudan.com/img/206-0.png)

</div>

# LeetCode 206：反转链表

题目链接：[反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)



## 题意

给出单链表的头节点 head，反转链表，返回反转后的链表。



## 示例

输入：head = [1, 2, 3, 4, 5]

输出：[5, 4, 3, 2, 1]



注意，如果是空链表



输入：head = []

输出：[]



## 提示

0 <= 链表节点数 <= 5000

-5000 <= Node.val <= 5000



# 题目解析

水题，难度简单。

题目没有思维上的难度，就是**把每个节点的 next 指向它的前驱节点即可**。

**主要考察臭宝写代码的能力**。



# 图解

这个题是将当前 next 节点的指针指向它的前驱节点，这就需要得有两个指针：一个指向当前节点，一个指向前驱节点。

<div align=center>

![206-1](https://cdn.codegoudan.com/img/206-1.png)

</div>

如果只有这俩的话又不对，因为当 p 的 next 指针指向前驱节点 q 的时候，上图就会变成这样：

<div align=center>

![206-2](https://cdn.codegoudan.com/img/206-2.png)

</div>

所以此时应该还需要一个临时指针 temp，保存当前节点的后继节点，也就是如下图：

<div align=center>

![206-3](https://cdn.codegoudan.com/img/206-3.png)

</div>

所以现在成了，先找到 temp，然后 p.next 再指向 q，最后 q，p 同时右移。

<div align=center>

![206-4](https://cdn.codegoudan.com/img/206-4.png)

</div>

如此反复，直至全部结束。

本方法遍历一遍链表，所以时间复杂度为 O(n)，需要了额外的 3 个指针，空间复杂度为 O(1)。



# 代码实现

Python 代码实现

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:

        # 空链表或者只有一个节点，直接返回
        if not head or head.next == None:
            return head

        # 初始化记录当前节点 p 和前驱节点 q
        p = head
        q = None

        while p:
            temp = p.next
            p.next = q
            q = p
            p = temp
            
        # 最后一步，p 肯定是指向 NULL，所以返回 q 作为新的头指针
        return q
```


