**交换链表**，和反转链表一样，也是必考的“老熟人”。

<div align=center>

![40d1f6698708b8161025dbcef5514d4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111149940_0.jpg)

</div>

# LeetCode 24：两两交换链表中的节点

题目链接：[两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)



## 题意

两两交换链表相邻节点的值，返回交换后的链表。



## 示例

输入：head = [1, 2, 3, 4]

输出： [2, 1, 4, 3]



## 提示

- 0 <= 链表节点数 <= 100

- 0 <= Node.val <= 100



# 题目解析

水题，难度中等。

这道题要求不能只是单纯的改变节点内部得值，需要进行实际的节点交换。

和反转链表一样，这类链表题思维上没有难度，**就是每次从链表上截取两个节点进行交换**。

**主要是考察代码实现能力**。

这道题我用**带虚拟头节点的单链表**实现。

虚拟头节点（其实我以前都叫头节点，后来有臭宝和我说容易看混，我就叫虚拟头节点），可能很多人叫做**哨兵节点**，放在第一个元素的节点之前，数据域一般没意义。



# 图解

先建立一个带虚拟头节点的单链表。

<div align=center>

![fda0fd241a014afc68089959308e254](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111532338_0.jpg)

</div>

PS：此处代码为 Python（”代码实现“小节处有 Java 代码，下同）。

```Python
# 链表节点类
class ListNode:
     def __init__(self, val=0, next=None):
         self.val = val
         self.next = next
# 创建虚拟节点
dummyHead = ListNode(-1)
```

因为每次要截取两个节点进行交换，初始建立 3 个指针 pre，p 和 q。

其中 pre 指向虚拟头节点，p 指向链表首节点，q 指向链表的第 2 个节点。

<div align=center>

![7ee9622db7124b90218b60be2a0f686](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111606636_0.jpg)

</div>

```Python
pre = dummyHead
p = head
q = p.next
```

节点两两交换主要分为 3 步。

第 1 步：pre 的后继指针 next 指向 q，即 pre.next = q。

<div align=center>

![362cfa820e6e73f6ce4046d588a5467](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111734210_0.jpg)

</div>

第 2 步：p 的后继指针 next 指向 q 的后继节点，即 p.next = q.next。

<div align=center>

![f8e21ee36710cce18725057b29db743](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111744087_0.jpg)

</div>

第 3 步：那就剩了 q 的后继指针 next 指向 p，即 q.next = p。

<div align=center>

![37ac5181295458e52536bfab55d4d3b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111753769_0.jpg)

</div>

所以第一次交换完，链表变成了：

<div align=center>

![eff82dc7b4c4d3a1fcdad18746660f9](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111809185_0.jpg)

</div>

```Python
pre.next = q
p.next = q.next
q.next = p
```

接下来就是 pre、p 和 q 指针同时右移。

```Python
pre = p
p = p.next
q = p.next
```

继续重复上述 3 步操作。

<div align=center>

![964b3ca54970a45e9d299d666669b23](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_111859410_0.jpg)

</div>

本方法遍历一遍链表，**时间复杂度为 O(n)，空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:

        # 空链表或者只有一个节点，直接返回
        if not head or head.next == None:
            return head

        # 创建虚拟节点
        dummyHead = ListNode(-1)

        # 初始化pre、p 节点
        pre = dummyHead
        p = head

        # 必须是节点数是偶数个的时候才能交换
        # 如果最后只剩下一个节点，即链表是奇数个节点，最后一个不用反转
        # 比如 head = [1,2, 3, 4, 5],输出 [2, 1, 4, 3, 5]
        while p and p.next:
            # 初始化 q 节点
            q = p.next

            # 交换节点 3 步走
            pre.next = q
            p.next = q.next
            q.next = p
            
            # 指针右移
            pre = p
            p = p.next
        
        # 返回链表
        return dummyHead.next
```



## Java 代码实现

```java
// 虚拟头结点
class Solution {
  public ListNode swapPairs(ListNode head) {

    ListNode dummyNode = new ListNode(0);
    dummyNode.next = head;
    ListNode prev = dummyNode;

    while (prev.next != null && prev.next.next != null) {
      ListNode temp = head.next.next; // 缓存 next
      prev.next = head.next;          // 将 prev 的 next 改为 head 的 next
      head.next.next = head;          // 将 head.next(prev.next) 的next，指向 head
      head.next = temp;               // 将head 的 next 接上缓存的temp
      prev = head;                    // 步进1位
      head = head.next;               // 步进1位
    }
    return dummyNode.next;
  }
}
```


