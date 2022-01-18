<div align=center>

![cd27836fa9a4141191e0f9fbdc04007](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_211824393_0.jpg)

</div>

# LeetCode 242：有效的字母异位词

题目链接：[有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)



## 题意

给定字符串 s 和 t，判断两个字符串是否为字母异位词。

ps：s 和 t 中每个字符出现的次数都相同，则 s 和 t 互为字母异位词。



## 示例

输入：s = "anagram", t = "nagaram"

输出：true



## 提示

- 1 <= s.length, t.length <= 5 * 10^4
- **s 和 t 仅包含小写字母**



# 题目解析

水题，难度简单，题目倒是挺唬人。

碰到这种类似什么字符出现次数啥的，如果没有思路了，都可以往哈希的思路上便一下。

关于哈希表，如果小婊贝们不懂的话，可以看我下面这篇文章，包会：



**[ACM 选手带你玩转哈希表！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921482&idx=1&sn=a0010596ea33c8605ed02b941cdfe854&chksm=ff22f7c7c8557ed1383751061be7d701e026a75ad0e22199356c826c05efcde955d99cccc4d5&scene=21#wechat_redirect)**



再来看关键点，这道题字符串 s 和 t 仅包含小写字母，小写字母就固定的 26个。

**这种定长的范围且和字符次数相关，用哈希妥妥的没问题。**

下面就是**找出哈希函数**，就是把每一个 key 对应到 0 ~ N-1 的范围内，并且放在合适的位置。

这里用的哈希函数 f(key) = key - ‘a’。这里其实是对字符 ASCII 的操作，字母 a ~ z 的 ASCII 是连续的 26 个数值。

比如 f(a) = 'a' - 'a' = 0，f(b) = 'b' - 'a' = 1，即字母 a 对应的是哈希表下标为 0 的位置，字母 b 对应的是哈希表下标为 1 的位置，剩下的依此类推。

哈希函数找好，下面的就很简单：

- 遍历字符串 s，哈希表对应的字符值加。
- 遍历字符串 t，哈希表对应的字符值减。
- 如果哈希表中的值都为 0，则 s 和 t 互为字母异位词。

具体的来看下面图解。



# 图解

在这以 s = "anagram", t = "nagaram" 为例。

首先初始化哈希表，根据哈希函数，字母 a ~ z 在哈希表中所占位置如下：

<div align=center>

![4078a59f985c0d73ecce3b6158bf064](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_212056551_0.jpg)

</div>

第一步，遍历字符串 s，碰到相应的字符，对应下标的哈希值 + 1。

对于 s = “anagram”，第 1 个字符为 a，a 对应哈希表下标为 0，则下标 0 对应值 +1：

<div align=center>

![328faf13c3f63a2a81b73dbfd6200d2](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_212114417_0.jpg)

</div>

```Python
# 对于字符串 s，在对应位置加（比如出现 a，就在 a 的位置 +1）
for i in range(len(s)):
    hash[ord(s[i]) - ord('a')] += 1
```

依次遍历完字符串 s，哈希表如下图：

<div align=center>

![a9af732985d6255a211564fa1a53566](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_212155653_0.jpg)

</div>

第二步，遍历字符串 t，碰到对应的字符，相应下标的哈希值 -1。

对于 t = "nagaram"，第 1 个字符为 n，n 对应的下标为 13，则下标 13 对应的哈希值 -1：

<div align=center>

![fd01bebfd24c5f063f0ae276724d6fa](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_212218485_0.jpg)

</div>

```Python
# 对于字符串 t，在对应位置减（比如出现 a，就在 a 的位置 -1）
for i in range(len(t)):
    hash[ord(t[i]) - ord('a')] -= 1
```

同样，依次遍历完字符串 t，哈希表如下图：

<div align=center>

![cdd0eb4d94e17384c2cbb838c5aca19](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_212253432_0.jpg)

</div>

第三步，遍历哈希表，所有的值均为 0，则 s 和 t 互为字母异位词，反之为否。

```Python
# 遍历哈希表，如果值都为 0，则为字母异位词
# 如果存在任一值不为 0 的哈希值，则不为字母异位词
for i in range(26):
    if hash[i] != 0:
                return False
return True
```

在此例中，字符串 s 和 t 互为字母异位词。

此题解法，**时间复杂度为 O(n)**，n 为 s 和 t 中较长的那个字符串的长度。

因为额外开了一个长为 26 的数组，所以**空间复杂度为 O(m)，m = 26**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        # 如果两个字符串长度不等，肯定不是字母异位词
        if len(s) != len(t):
            return False
        # 如果两个字符串的长度相等
        # 初始化哈希函数，字符串只包含小写字母，故初始化 26 个
        hash = [0] * 26

        # 循环两个字符串
        # 对于字符串 s，在对应位置加（比如出现 a，就在 a 的位置 +1）
        for i in range(len(s)):
            hash[ord(s[i]) - ord('a')] += 1

        # 对于字符串 t，在对应位置减（比如出现 a，就在 a 的位置 -1）
        for i in range(len(t)):
            hash[ord(t[i]) - ord('a')] -= 1
            
        # 遍历哈希表，如果值都为 0，则为字母异位词
        # 如果存在任一值不为 0 的哈希值，则不为字母异位词
        for i in range(26):
            if hash[i] != 0:
                return False

        return True
```



## Java 代码实现

```Java
class Solution {
    public boolean isAnagram(String s, String t) {

        int[] record = new int[26];
        for (char c : s.toCharArray()) {
            record[c - 'a'] += 1;
        }
        for (char c : t.toCharArray()) {
            record[c - 'a'] -= 1;
        }
        for (int i : record) {
            if (i != 0) {
                return false;
            }
        }
        return true;
    }
}
```



---

**图解字母异位词**到这就结束辣，这是哈希表实战系列第一道题，算是开胃菜。

**AC 不是目的，目的是好好去琢磨琢磨这种解题方法，然后能运用到之后的题目中。**
