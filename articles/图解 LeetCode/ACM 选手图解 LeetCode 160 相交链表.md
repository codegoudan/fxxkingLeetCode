大家好呀，我是直男蛋。

今天来做**相交链表**，这道题本来是**难度简单**的水题，但是有两个奇怪点把我给整楞了。

一个就是题意，另一个就是解题方式，感觉直接把我的直男属性给暴露了。

具体的到下面细聊，现在先开始看题。

![ebfe8f04df82891c77f29b869f7f753](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_153627068_0.jpg)



# LeetCode 160：相交链表

题目链接：[相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)



## 题意

给出两个单链表的头节点，找出两个单链表相交的起始节点。



## 示例

![e1558939fd5da6eaf1dcc83a84aa50a](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_153948470_0.jpg)



## 提示

题目保证不存在环。

- len(listA) = m, len(listB) = n

- 0 <= m, n <= 3 * 10^4

- 1 <= Node.val <= 10^5

- 0 <= skipA <= m, 0 <= skipB <= n



# 题目解析

在说我的解法之前，我先来说一下文章开头我说的是啥意思。

这个题目一开始题意给我看楞了，不看示例，打眼看上去还以为是找数值相等的节点。

估计很多臭宝容易看懵，**这道题其实是找相交节点的指针**（即同一节点，引用完全相同），而不是单纯遍历找第一个数值相同的节点，这点大家要搞清楚。

光这个出题的题意，我感觉就得值个难度 hard！

![bf8421e47e72f8a3724ad76bdb37134](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_154043261_0.jpg)

其实搞清楚了题意，稍微一想解法就能出来。

在我劈里啪啦的敲完代码然后 AC 的时候，随意点开了力扣的题解区，官方的题解给我看楞了。

虽然用了双指针，但是它的解法楞是用上了数学的思想。怕你们在做的时候会先去看题解，然后又看不懂别人怎么个思想，所以这里就给臭宝们讲一下**“浪漫”的做法**。

![1266ec8549c864358f70b0c8ab7c9f1](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_154105493_0.jpg)

官方的方法是：

- 链表 A 的指针 flagA 从链表 A 的头节点开始遍历完链表 A，再遍历链表 B
- 链表 B 的指针 flagB 从链表 B 的头节点开始遍历完链表 B，再遍历链表 A

这样的话就消除了链表 A 和 B 的长度差。

假设链表 A 长度为 a，链表 B 长度为 b，两个链表相交的部分长度为 c。

当走到相交节点时：

- flagA 走过的长度为：a + (b - c)
- flagB 走过的长度为：b + (a - c)

这种做法，不管两个指针是否相交，他们走过的长度都是一样的，所以就有了公式：
$$
a + (b - c) = b + (a - c)
$$
**若两个链表能相交**，那么 c > 0，两个指针同时指向相交节点。

**若两个链表不相交**，那么 c = 0，两个指针同时指向 Null。

是不是很巧妙的做法，**时间复杂度 O(n+m)，空间复杂度为 O(1)**。

看了一下评论，浪漫气息弥漫：



> 错的人就算走过了对方的路也还是会错过。
>
> 
>
> 走到尽头见不到你，于是走过你来时的路，等到相遇时才发现，你也走过我来时的路。


看到这我应该非常肯定自己是个废物了。

本应该拍手称快，但是这解法属实让我愣了：这还是一道简单题？

我感觉我等凡人，不在草稿纸上划拉划拉，一下子也看不懂它的题解是什么意思，更不用说在面试或者比赛中想出这样的解法了。

好了，这个解法就说到这吧。

下面，该是本直男蛋的做法了，比起人家的解法我这简直是俗不可耐，但保证看一眼就能看明白。

我也用到了双指针，不过我是从屁股开始的。

![61ab1d3b13faf51fe8777586d7800d9](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_154227320_0.jpg)

你想，**如果两个链表相交的话，那必定有一段公共段**。

既然有这个前提，那公共段以前的对比那是白送的无用功。

**让长链表先走“两个链表长度的差值”，然后链表从长度相同的位置开始同步向后遍历。**

**找到相交节点，则返回节点，如果没相交节点的话，两个指针都指向 Null。**

光这么说可能不好理解，下面来看图解。



# 图解

我以 listA = [0, 9, 1, 2, 4]、listB = [3, 2, 4] 为例，相交节点的值为 2。

指向 listA 和 listB 的指针分别为 flagA 和 flagB，两个链表的长度分别用 lenA 和 lenB 表示。

首先初始化两个指针和链表长度。

![52446d6140120371ee1bcadcedf43f4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_154308997_0.jpg)

```Python
# 初始化headA、headB的指针和长度
flagA, flagB = headA, headB
lenA, lenB = 0, 0
```

然后计算两个链表长度的差值

```Python
# 求链表 A 的长度
while flagA:
    flagA = flagA.next
    lenA += 1
# 求链表 B 的长度
while flagB:
    flagB = flagB.next
    lenB += 1
```

在本例中，链表 A 比链表 B 长，差值为 2，所以链表 B 的指针 flagB 移动差值的距离。

![ce7457cfcbbd47078b220a688129e03](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_154403495_0.jpg)

```Python
# 当A是长链表，则指针flagA后移到和B链表同等长度的位置上。
if lenA > lenB:
    d_value = lenA - lenB
    while d_value:
        flagA = flagA.next
        d_value -= 1
```

之后 flagA 和 flagB 同时向后遍历。

```Python
flagA = flagA.next
flagB = flagB.next
```

如果找到 flagA 和 flagB 指向相同，即相遇，则返回指针，否则继续遍历，直到两个指针都指向 Null，表明没有交点。

本例中相遇节点为 2。

![41131b4fc18894807440a72c7f92b52](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_154501500_0.jpg)

```Python
 if flagA == flagB:
      return flagA
```

我这个解法比起人家那个浪漫的确实 low 了点，但是复杂度上完全不输。

链表 headA 和 headB 各遍历了一遍，**时间复杂度同样为 O(n + m)**。

同时也没整额外的存储，**空间复杂度为 O(1)**。

我要骄傲了呀。

![5e3f024c0ae1c3ec40f3139411b0b69](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_154530733_0.jpg)



# 代码实现



## Python 代码实现

为了让大家看明白点，代码我用了最简单粗暴的形式写的，一点炫技都没加，保证能看懂。

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:

        # 链表A和B任一为空，链表不相交
        if not headA or not headB:
            return None

        # 初始化headA、headB的指针和长度
        flagA, flagB = headA, headB
        lenA, lenB = 0, 0

        # 求链表 A 的长度
        while flagA:
            flagA = flagA.next
            lenA += 1

        # 求链表 B 的长度
        while flagB:
            flagB = flagB.next
            lenB += 1

        # 重新指向表头
        flagA, flagB = headA, headB

        # 为了让大家看的明白点，我就不用华丽花哨的写法了，用最笨的写法表示。
        # 当A是长链表，则指针flagA后移到和B链表同等长度的位置上。
        if lenA > lenB:
            d_value = lenA - lenB
            while d_value:
                flagA = flagA.next
                d_value -= 1
        # 当B是长链表，则指针flagB后移到和A链表同等长度的位置上。
        else:
            d_value = lenB - lenA
            while d_value:
                flagB = flagB.next
                d_value -= 1

        # 然后两个指针flagA和flagB同时遍历
        while flagA:
            if flagA == flagB:
                return flagA
            else:
                flagA = flagA.next
                flagB = flagB.next

        # 如果没有相遇，返回 None
        return None
```



## Java 代码实现

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode curA = headA;
        ListNode curB = headB;
        int lenA = 0, lenB = 0;
        while (curA != null) { // 求链表A的长度
            lenA++;
            curA = curA.next;
        }
        while (curB != null) { // 求链表B的长度
            lenB++;
            curB = curB.next;
        }
        curA = headA;
        curB = headB;
        // 让curA为最长链表的头，lenA为其长度
        if (lenB > lenA) {
            //1. swap (lenA, lenB);
            int tmpLen = lenA;
            lenA = lenB;
            lenB = tmpLen;
            //2. swap (curA, curB);
            ListNode tmpNode = curA;
            curA = curB;
            curB = tmpNode;
        }
        // 求长度差
        int gap = lenA - lenB;
        // 让curA和curB在同一起点上（末尾位置对齐）
        while (gap-- > 0) {
            curA = curA.next;
        }
        // 遍历curA 和 curB，遇到相同则直接返回
        while (curA != null) {
            if (curA == curB) {
                return curA;
            }
            curA = curA.next;
            curB = curB.next;
        }
        return null;
    }
    
}
```
