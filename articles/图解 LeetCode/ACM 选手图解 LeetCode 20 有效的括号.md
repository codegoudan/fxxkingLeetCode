**判断括号是否有效**，常见题。



![800b54ae3d38699d34a6850c59e32f2](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163457389_0.jpg)



# LeetCode 20：有效的括号

题目链接：[有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)



## 题意

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。



## 示例

输入：s = "()[]{}"

输出：true



输入：s = "([)]"

输出：false



## 提示

有效字符串需满足：

- 左括号必须用相同类型右括号闭合
- 左括号必须以正确顺序闭合

1 <= s.length <= 10^4

s 仅由括号 () [] {} 组成



# 题目解析

**括号匹配是使用栈解决的经典问题**，难度简单。

还不太会的臭宝看下面链接，本蛋写的：

**[呔！“栈”住，队列！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475919772&idx=1&sn=8574188f72d892d501f15956b6a2a629&chksm=ff22ee11c8556707dd6d5af977125ce6698ebbcfaa3b9083e14c3658d2975761079a9c2082a4&scene=21#wechat_redirect)**

看会了栈，这道题基本就会了大半，剩下小半就是动动小脑袋瓜，稍微思考。

这题就是**维护一个栈，碰到左括号就入栈，碰到右括号，取出栈顶元素，判断栈顶元素和右括号是否匹配。**

如果不匹配，返回 false，如果匹配则重复上述步骤，直到遍历完字符串。

需要注意的是，有**两种特殊情况**也是返回 false：

- 遍历完整个字符串，栈不为空，说明多了左括号。
- 碰到右括号，但此时栈为空，说明多了右括号。

抛除上面的情况，其余的都返回 true。



# 图解

假设 s = "()[]{}"

首先初始化。

```Python
# 初始化
stack = []

pairs = {
    ')': '(',
    ']': '[',
    '}': '{'
}

```

根据“题目解析”中说的，碰到左括号入栈，所以第 1 步，"(" 入栈

![dbfae677c5743da82cc10f22996acf3](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163706223_0.jpg)

```Python
for i in s:
    # 如果是左括号，压入栈
    if i not in pairs:
        stack.append(i)
```

接下来碰到右括号 ")"，取出栈顶元素 "("，判断栈顶元素是否和当前右括号配对，结果发现配对。

![726e46f80b802f35ae773f65e780e96](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163742461_0.jpg)

既然匹配，那就继续进行下 1 步，接下来碰到左括号 "["，入栈。

![af05284c5bc67f124a056e26069df18](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163802435_0.jpg)

接下来碰到右括号 "]"，取出栈顶元素 "["，判断栈顶元素是否和当前右括号配对，结果发现配对。

![4c1acb1bf9571ffe7e1cff40652095a](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163829144_0.jpg)

既然又匹配，那就继续进行下 1 步，接下来碰到左括号 "{"，入栈。

![8443419385ca8d34d667ac39f22405a](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163849155_0.jpg)

接下来碰到右括号 "}"，取出栈顶元素 "{"，判断栈顶元素是否和当前右括号配对，结果发现配对。

![d3fedaddcc21517ef00bec28b1444e2](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163907396_0.jpg)

此时字符串全部遍历完，同时栈也为空，字符串 s 有效，返回 true。

```Python
 return not stack
```

再来假设 s = "([)]"

前两个元素都是左括号，所以直接入栈。

![b0e680768370c80ca081609a344506b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_163953179_0.jpg)

啥接下来碰到右括号 ")"，取出栈顶元素 "["，栈顶元素与右括号不匹配，返回 false。

![afb94fb6342da1b1113d30244bb3734](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_164013549_0.jpg)

```Python
# 在遍历过程中，碰到空栈或者括号不匹配，返回 False。
elif not stack or pairs[i] != stack.pop():
    return False
```

每个元素的进栈或出栈的操作都是 O(1)，最坏情况下，每个元素都会进且仅进栈 1 次，所以**时间复杂度为 O(1) \* n = O(n)**。

同理，最坏情况下，是所有的元素都入栈，所以**空间复杂度也为 O(n)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def isValid(self, s: str) -> bool:

        # 字符串有奇数个元素，不匹配
        if len(s) % 2 == 1:
            return False

        # 初始化栈
        stack = []

        pairs = {
            ')': '(',
            ']': '[',
            '}': '{'
        }

        for i in s:
            # 如果是左括号，压入栈
            if i not in pairs:
                stack.append(i)
            # 在遍历过程中，碰到空栈或者括号不匹配，返回 False。
            elif not stack or pairs[i] != stack.pop():
                return False

        # 全部遍历完，如果栈为空，返回 True，栈不为空，返回 False
        return not stack
```



## Java 代码实现

```Python
class Solution {
    public boolean isValid(String s) {
        Deque<Character> deque = new LinkedList<>();
        char ch;
        for (int i = 0; i < s.length(); i++) {
            ch = s.charAt(i);
            //碰到左括号，就把相应的右括号入栈
            if (ch == '(') {
                deque.push(')');
            }else if (ch == '{') {
                deque.push('}');
            }else if (ch == '[') {
                deque.push(']');
            } else if (deque.isEmpty() || deque.peek() != ch) {
                return false;
            }else {//如果是右括号判断是否和栈顶元素匹配
                deque.pop();
            }
        }
        //最后判断栈中元素是否匹配
        return deque.isEmpty();
    }
}
```


