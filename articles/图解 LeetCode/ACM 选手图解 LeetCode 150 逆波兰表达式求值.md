**逆波兰表达式求值**，可能臭宝们一看“逆波兰表达式”这个洋气的名字，下意识的会觉得难搞。

其实这个词只是看着唬人，就是纸老虎。

逆波兰表达式又叫**后缀表达式，是将运算符写在操作数之后**。

像我们平时写的是类似 a + b 这种表达式，这叫**中缀表达式**，符合我们作为人的思维方式，而后波兰表达式则是写成 ab+。

简单说下为啥要弄个逆波兰表达式这个玩意。

原因就是从我们人的角度来看，简单的一批的**中缀表达式，在计算机看来，简直是复杂的无与伦比，因为计算机普遍采用的是栈式结构，执行的是先入后出的操作，所以逆波兰式在它眼里才是个正常的娃儿**。

<div align=center>

![150-0](https://cdn.codegoudan.com/img/150-0.png)

</div>

# LeetCode 150：逆波兰表达式求值

题目链接：[逆波兰表达式求值](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)



## 题意

根据逆波兰表示法，求表达式的值。

有效算符包括 + - * /。运算对象可以是整数，可以是其他逆波兰表达式。



## 示例

输入：tokens = ["2", "1", "+", "3", "*"]

输出：9

解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9



输入：tokens = ["4","13","5","/","+"]

输出：6

解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6



## 提示

整数除法只保留整数部分。

给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

- 1 <= tokens.length <= 104

- tokens[i] 要么是一个算符（"+"、"-"、"*" 或 "/"），要么是一个在范围 [-200, 200] 内的整数。



# 题目解析

水题，难度中等，感觉难度都堆在“逆波兰表达式”这几个字上。

逆波兰表达式主要有以下两个优点：

- 去掉括号后表达式无歧义，上式即便写成 1 2 + 3 4 + * 也可以依据次序计算出正确结果。

- 适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中。

**在前面说了，**逆波兰表达式为计算机这种思维而生**，那这个逆波兰表达式求值显然也是用栈来解决。

逆波兰表达式严格遵守从左到右的运算，所以解法也很简单，其实就是维护一个栈，从左到右扫描 tokens：

- 如果**遇到数**，数入栈。
- 如果**遇到操作符**，栈顶两个元素出栈，先出栈的数在运算符右侧，后出栈的在运算符左侧，运算完后，将运算值入栈。

等全部扫描完以后，**栈内剩下的那个数，就是最后的结果**。



# 图解

假设 tokens = ["2", "1", "+", "3", "*"]。

首先维护一个栈，根据“题目解析”中说的，碰到数先入栈，那第 1 步和第 2 步，数字 2 和数字 1 入栈。

<div align=center>

![150-1](https://cdn.codegoudan.com/img/150-1.png)

</div>

```Python
# 初始化栈
stack = []
for c in tokens:
    # 如果当前字符不是操作符，入栈
    if c not in ['+', '-', '*', '/']:
        stack.append(int(c))
```

接下来第 3 步，当前元素为操作符，所以栈顶两个元素出栈，先出栈的在操作符右侧，后出栈的在操作符左侧，运算完，新值入栈。

<div align=center>

![150-2](https://cdn.codegoudan.com/img/150-2.png)

</div>

```Python
# 先出栈的数在操作符右侧
right = stack.pop()
# 后出栈的数在操作符左侧
left = stack.pop()
# 进行相应运算，运算完的数值入栈
if c == '+':
    stack.append(left + right)
```

往下第 4 步，是数字，入栈。

<div align=center>

![150-3](https://cdn.codegoudan.com/img/150-3.png)

</div>

最后第 5 步，碰到操作符，出栈，相乘，入栈。

<div align=center>

![150-4](https://cdn.codegoudan.com/img/150-4.png)

</div>

```Python
# 进行相应运算，运算完的数值入栈
if c == '+':
    stack.append(left + right)
elif c == '-':
    stack.append(left - right)
elif c == '*':
    stack.append(left * right)
else:
    # 除法这注意一下
    stack.append(int(left / right))
```

至此，结束，返回栈中的值即可。

```Python
# 最后只剩下一个数值，就是最终结果
return stack[0]
```

因为这个只需要从左到右遍历 tokens 数组一次，所以**时间复杂度为 O(N)**。

做的过程中维护了一个栈存储计算过程中的数，所以**空间复杂度为 O(N)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        # 初始化栈
        stack = []

        for c in tokens:
            # 如果当前字符不是操作符，入栈
            if c not in ['+', '-', '*', '/']:
                stack.append(int(c))
            # 如果当前字符是操作符
            else:
                # 先出栈的数在操作符右侧
                right = stack.pop()
                # 后出栈的数在操作符左侧
                left = stack.pop()

                # 进行相应运算，运算完的数值入栈
                if c == '+':
                    stack.append(left + right)
                elif c == '-':
                    stack.append(left - right)
                elif c == '*':
                    stack.append(left * right)
                else:
                    # 除法这注意一下
                    stack.append(int(left / right))
        # 最后只剩下一个数值，就是最终结果
        return stack[0]
```



## Java 代码实现

```Java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stack = new LinkedList<Integer>();
        int n = tokens.length;
        for (int i = 0; i < n; i++) {
            String token = tokens[i];
            if (isNumber(token)) {
                stack.push(Integer.parseInt(token));
            } else {
                int num2 = stack.pop();
                int num1 = stack.pop();
                switch (token) {
                    case "+":
                        stack.push(num1 + num2);
                        break;
                    case "-":
                        stack.push(num1 - num2);
                        break;
                    case "*":
                        stack.push(num1 * num2);
                        break;
                    case "/":
                        stack.push(num1 / num2);
                        break;
                    default:
                }
            }
        }
        return stack.pop();
    }
```



