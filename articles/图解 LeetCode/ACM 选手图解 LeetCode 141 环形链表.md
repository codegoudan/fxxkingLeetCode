**环形链表**题，依然是**面试高频题**。

判断环形链表在思维上稍微复杂一点儿，但是不慌，有我在。

<div align=center>

![4ad921bd0dfdc16b7746dfb9531f888](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_112458912_0.jpg)

</div>



# LeetCode 141：环形链表

题目链接：[环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)



## 题意

给定一个链表，判断链表中是否有环。



## 示例

使用整数 pos 表示链表尾连接到链表中的位置(索引从 0 开始)。



输入：head = [3, 2, 0,  -4]，pos = 1

输出：true

解释：链表中有一个环，其尾部连接到第 2 个节点。



输入：head = [1]，pos = -1

输出：false

解释：链表中没有环。



## 提示

- pos **不作为参数进行传递**，仅标识链表的实际情况。

- 0 <= 链表节点数 <= 10^4

- 10^-5 <= Node.val <= 10^5

- pos 为 -1 或者链表中的一个有效索引。



# 题目解析

难度简单，解题方法古怪，在实际应用中应该没人脑子瓦特用这种方法。

这种古怪的解题方法就是：**快慢指针**。

**快慢指针，顾名思义，是使用速度不同的指针（可用在链表、数组、序列等上面），来解决一些问题。**

这些问题主要包括：

- 处理环上的问题，比如环形链表、环形数组等。
- 需要知道链表的长度或某个特别位置上的信息的时候。

快慢指针这种算法证明，它们肯定是会相遇的，快的指针一定会追上慢的指针，可以理解成操场上跑步，跑的快的人套圈跑的慢的人。

一般用 fast 定义快指针，用 slow 定义慢指针。
速度不同是指 fast 每次多走几步，slow 少走几步。一般设定的都是 fast 走 2 步，slow 走 1 步。

当然设置成别的整数也是可以的，比如 fast 走 3 步，slow 走 1 步。



# 图解

在本题中使用快慢指针：

- 若是链表**无环**，那么 fast 指针会先指向 Null。
- 若是链表**有环**，fast 和 slow 迟早会在环中相遇。

以 head = [3, 2, 0, -4]，pos = 1 为例。

第 1 步：初始化快慢指针。每次 slow 走 1 步，fast 走 2 步。

<div align=center>

![5b6362643f4341632a6b7955b342a61](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_112706876_0.jpg)

</div>

```Python
 # 初始化快慢指针
 fast = slow = head
```

第 2 步：slow 移动 1 步，fast 移动 2 步。

<div align=center>

![cf0cd4e6e506bfb5698099303263251](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_112801220_0.jpg)

</div>

```Python
while fast and fast.next:
       # 快指针移动 2 步，慢指针移动 1 步
       fast = fast.next.next
       slow = slow.next
```

第 3 步：同上。

<div align=center>

![7fab6a256c13f99a2009a11d21b9a33](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_112853832_0.jpg)

</div>

第 4 步：同上。

<div align=center>

![f9bc577148fdcbcd8a8d3700246c0b4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_112911125_0.jpg)

</div>

此时 fast = slow，判断有环，返回 true，结束。

```Python
# 快慢指针相遇，有环
if fast == slow:
    return True
```

快慢指针解本题：

- 链表无环，每个节点至多访问 2 次。
- 链表有环，最多移动 n 轮。

所以**时间复杂度为 O(n)**。

除此以外，只用了额外的 fast 和 slow 两个指针，所以**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:

        # 空链表或链表只有一个节点，无环
        if not head or head.next == None:
            return False

        # 初始化快慢指针
        fast = slow = head

        # 如果不存在环，肯定 fast 先指向 null
        # 细节：fast 每次走 2 步，所以要确定 fast 和 fast.next 不为空，不然会报执行出错。
        while fast and fast.next:

            # 快指针移动 2 步，慢指针移动 1 步
            fast = fast.next.next
            slow = slow.next

            # 快慢指针相遇，有环
            if fast == slow:
                return True

        return False
```



## Java 代码实现

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
/*算法思想：使用快慢指针相遇解决是否存在环的问题*/
public class Solution 
{
    public boolean hasCycle(ListNode head) 
    {
        if(head==null)//头指针为空
            return false;
        ListNode fast=head.next;
        ListNode slow=head;
        while(fast!=null&&fast.next!=null)
        {
            fast=fast.next.next;//快指针
            slow=slow.next;//慢指针
            if(fast==slow)//相遇，则一定存在环，否则慢指针根本跟不上快指针
                return true;
        }
        return false;
    }
}
```



