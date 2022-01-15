<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220115_221611704_20.jpg" alt="88fee799034104315b5988bde33ee31"  />

</div>

# LeetCode 1047：删除字符串中的所有相邻重复项

题目链接：[删除字符串中的所有相邻重复项](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/)



## 题意

小写字母组成字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。



## 示例

输入：abbaca

输出：ca



## 提示

答案保证唯一

- 1 <= S.length <= 20000

- S 仅由小写英文字母组成



# 题目解析

水题，难度简单。

**匹配到相邻的两个元素是相同的就消除**，你看有没有眼熟，像不像是括号匹配那道题。

呃，如果你不知道括号匹配那道题，看下面链接咧，我写过。



[栈，你告诉我这个括号配不配！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475920000&idx=1&sn=4d94d8c1fc33e43940a253c50f130252&chksm=ff22ed0dc855641baf002c72356fe58beb233e9dd9ff01875deab483728c2ebd5bf73f353e89&scene=21#wechat_redirect)


这种类型的题都算是匹配问题，**只要是匹配问题，大家记住，在没有思路的时候，都可以考虑用栈碰一碰**。

既然看透了这道题的本质，那做法也就呼之欲出了。

**维护一个栈**，从左到右依次扫描字符串，让当前字符和栈顶元素比较：

- 如果**相同**，证明当前两个元素相邻且相同或者由于之前删除了相同元素导致间接相邻且相同，此时栈顶元素出栈，当前字符不入栈。
- 如果**不同**，则当前元素入栈。

如此反复，直至扫描结束，栈里剩下啥字符就是啥字符。

需要注意的是，**删除两个相邻且相同的字符可能会产生新的相邻且相同的字符**。



# 图解

假设字符串 S = abbaca

首先初始化栈

```Python
# 初始化栈
stack = []
```

根据“题目解析”中说的，如果当前元素与栈顶元素不同，则当前元素入栈。

所以第 1 步，当前元素为 a，栈为空，所以元素 a 入栈。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_170036175_0.jpg" alt="6ff8db8534a48f0da8247d21a384e8e"  />

</div>

第 2 步，当前元素为 b，此时栈顶元素为 a，两者不相等，所以元素 b 入栈。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_170052282_0.jpg" alt="1196118d8fba6357150739c12ce1165"  />

</div>

接下来第 3 步，当前元素为 b，栈顶元素为 b，两者相等，栈顶元素出栈。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_170107813_0.jpg" alt="22af199d53f7e87d501f57ab54964e6"  />

</div>

```Python
for char in s: 
    if stack and stack[-1] == char:
        stack.pop()
```

之后第 4 步，当前元素为 a，栈顶元素为 a，这就是由于删除两个相邻且相同的字符，产生了新的相邻且相同的字符。

<div align=center>

![d572722e0769d45a0127719121695f7](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_170138102_0.jpg)

</div>

最后两步没有相同的，直接入栈，输出栈内元素即可。

在这提醒一下**编程语言直接用“栈”这个结构的，输出的时候要反转下字符串**。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_170156784_0.jpg" alt="2b7819f489cfcdb21dc3b134d907974"  />

</div>

因为从头到尾只遍历字符串 S 一次，所以**时间复杂度为 O(n)**。

在这个过程中，维护了一个额外的栈，所以本解法**空间复杂度为 O(n)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        # 初始化栈
        stack = []

        for char in s:
            # 如果"栈不为空"且"栈顶元素和当前元素相同"则栈顶元素出栈
            if stack and stack[-1] == char:
                stack.pop()
            # 否则当前元素进栈
            else:
                stack.append(char)
        
        # 栈内元素拼接成一个字符串
        return ''.join(stack)
```



## Java 代码实现

此处直接拿字符串当栈，省去了栈要转为字符串的操作。

```Python
class Solution {
    public String removeDuplicates(String s) {
        // 将 res 当做栈
        StringBuffer res = new StringBuffer();
        // top为 res 的长度
        int top = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            // 当 top > 0,即栈中有字符时，当前字符如果和栈中字符相等，弹出栈顶字符，同时 top--
            if (top >= 0 && res.charAt(top) == c) {
                res.deleteCharAt(top);
                top--;
            // 否则，将该字符 入栈，同时top++
            } else {
                res.append(c);
                top++;
            }
        }
        return res.toString();
    }
}
```



---

这道题就很能说明问题，其实就是换汤不换药，穿上个马甲也不能装乌龟。

**大家在做题的时候还是得仔细琢磨，不能过去了就是过去了**。

