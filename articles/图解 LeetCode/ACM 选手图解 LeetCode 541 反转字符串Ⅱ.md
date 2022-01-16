**反转字符串Ⅱ**，是在上篇简单到扣脚的反转字符串的基础上，加了一丢丢的限制条件。

![fa7d4c6a0351a337d3225945b2448ac](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_224351920_0.jpg)



# LeetCode 541：反转字符串Ⅱ

题目链接：[反转字符串Ⅱ](https://leetcode-cn.com/problems/reverse-string-ii/)



## 题意

我给定一个字符串 s 和一个整数 k，从开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。



如果剩余字符少于 k 个，则将剩余字符全部反转。

如果剩余字符小于 2k 个，则反转前 k 个字符，其余字符保持原样。



## 示例

输入：s = "abcdefg"，k = 2

输出："bacdfeg"



## 提示

- 1 <= s.length <= 10^4
- s 仅由小写英文组成
- 1 <= k <= 10^4



# 题目解析

水题，难度简单。

题意还是很好理解的，一句话说就是：**每次隔着 k 个字符反转 k 个字符**。

字符串的反转，在上道题字符串反转中写过，链接在下面：



[ACM 选手图解 Leetcode 反转字符串](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475920931&idx=1&sn=cf73562d5c06df758f75d22a5e3a7e13&chksm=ff22e9aec85560b84c7aa861dbff4038007a530109d9c6bd698493c7314e9787a3cca5ba9231&scene=21#wechat_redirect)



重点就是将 s[0] s[1] ... s[n-1] 以 s[n-1] ... s[1] s[0] 的形式输出。

即 s[i] 和 s[n - i - 1] 交换位置。

可以使用**双指针**的方式解决字符串反转，维护一个左指针 left 和右指针 right。

初始化 left 指向数组首元素，right 指向数组右元素。

当 left < right 的时候，交换 s[left] 和 s[right]，同时 left 右移，right 左移。

<div align=center>

![f211e282ed84022b934844445507c3b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_224403782_0.jpg)

</div>

这道题就是直接**按照题意模拟**就好了，就是**注意一下边界问题**，也就是最后剩余的字符块：

- 剩余字符块的长度 0 <= length <= k 时，不满足 k 个字符，当前字符直接全部反转。
- 当前字符块的长度 k <= length <= 2k，那前 k 个字符是完整的，只反转前 k 个字符。



# 图解

以 s = "abcdefg"，k = 2 为例。

按照要求，每次计数至 2k 个字符，则在此，2 * k = 4。

字符串 s 相当于被分为了两部分 "abcd" 和 "efg"。

<div align=center>

![2e6643cbf299b120f7a79a4ad2d8ef8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_224416486_0.jpg)

</div>

既然是模拟，直接看图就好了。

在这我先写了个反转字符串函数：

```Python
# 反转字符串函数
def reverseString(self,s):
    # 初始化双指针
    left = 0
    right = len(s) - 1
    # 这种方法可以不用判断元素奇偶
    while left < right:
        s[left], s[right] = s[right], s[left]
        #交换后，左指针右移，右指针左移
        left += 1
        right -= 1
    return s
```

第 1 步：

<div align=center>

![3270089578ac7ed8d891b7010d28df1](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_224425033_0.jpg)

</div>

```python
for i in range(0, len(s), 2 * k):
    res[i : i + k] = self.reverseString(res[i : i + k])
```

第 2 步：就是题目解析中说的剩余的字符块，此时的字符块长度大于 k 且小于 2k，所以只反转前 k 个字符。

<div align=center>

![1fa44f852998c88085a6bf859b62054](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211216_224434389_0.jpg)

</div>

本道题的解法**时间复杂度依然为 O(n)**。

至于空间复杂度依照你所用的编程语言来确定，因为有的编程语言存在字符串不可修改的情况，比如 Python，这就需要额外用一个数组来存储，那么它的**空间复杂度就成了 O(n)**。

如果所用编程语言字符串支持修改，那**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def reverseString(self,s):
        # 初始化双指针
        left = 0
        right = len(s) - 1

        # 这种方法可以不用判断元素奇偶
        while left < right:
            s[left], s[right] = s[right], s[left]

            #交换后，左指针右移，右指针左移
            left += 1
            right -= 1

        return s

    # 列表反转
    def reverseStr(self, s: str, k: int) -> str:
        res = list(s)

        for i in range(0, len(s), 2 * k):
            res[i : i + k] = self.reverseString(res[i : i + k])

        return ''.join(res)
```



## Java 代码实现

```java
class Solution {
    //翻转数组
     public void reverseString(char[] s,int left,int right) {
        //左右双指针
        while(left<right){
            char temp = s[right];
            s[right] = s[left];
            s[left] = temp;
            //移动指针
            left++;
            right--;
        }
    }

    public String reverseStr(String s, int k) {
        //字符串转换成char数组
        char[] ar = s.toCharArray();
        int len = ar.length;
        //i作为左指针进行遍历循环，每次移动两个k长度，就是准备反转部分的起点
        for(int i =0;i<len; i+=2*k){
            //取数组长度和反转终止点的最小值，就不用分情况讨论
            reverseString(ar,i,Math.min(i+k,len)-1);
        }
        //数组转换成字符串
        return new String(ar);
    }
}
```


