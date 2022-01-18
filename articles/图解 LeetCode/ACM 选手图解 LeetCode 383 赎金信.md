<div align=center>

![8adedb2e33ff55136355c5e2d6a5ea4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_213244801_0.jpg)

</div>



# LeetCode 383：赎金信

为了不暴露赎金信的字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。

题目链接：[赎金信](https://leetcode-cn.com/problems/ransom-note/)



## 题意

给定一个赎金（ransom）字符串和一个杂志（magazine）字符串。

判断 ransom 能不能由 magazine 里的字符串构成。可以返回 True，否则返回 False。



## 示例

输入：ransomNote = "anagram", magazine = "nagaram"

输出：true



## 提示

- **你可以假设两个字符串均只含有小写字母。**



# 题目解析

水题，难度简单。

我发现力扣上好多题乍一看标题会觉的：“卧槽？看这个标题就知道我不会做”。

等看到题意，心理活动就成了：“哈？就这？”。

可能这就是...下马威？

<div align=center>

![b627520e412645cab28af757f7c72bd](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_213358578_0.jpg)

</div>

这道题和之前做过的“有效的字母异位词”很像，忘记的可以看下文章：



**[ACM 选手图解力扣之有效的字母异位词！](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921505&idx=1&sn=edf8b3420e0b1bf406f98a990074bbe0&chksm=ff22f7ecc8557efa3edfac0d6f107b67bffca87d5debb474e1d33b7b12a7c73ba3b756a280d1&scene=21#wechat_redirect)**



有效的字母异位词是求字符串 s 和字符串 t 是否可以相互组成。

这道赎金信是求字符串 magazine 是否能组成字符串 ransomNote，不用管字符串 ransomNote 是否能组成字符串 magazine。

明白了这些，再来看关键点**两个字符串均只含有小写字母**，小写字母就固定的 26 个。

**这种定长的范围且和字符次数相关，用哈希妥妥的没问题。**

下面就是**找出哈希函数**，就是把每一个 key 对应到 0 ~ N-1 的范围内，并且放在合适的位置。

这里用的哈希函数 f(key) = key - ‘a’。这里其实是对字符 ASCII 的操作，字母 a ~ z 的 ASCII 是连续的 26 个数值。

比如 f(a) = 'a' - 'a' = 0，f(b) = 'b' - 'a' = 1，即字母 a 对应的是哈希表下标为 0 的位置，字母 b 对应的是哈希表下标为 1 的位置，剩下的依此类推。

哈希函数找好，下面的就很简单：

- 遍历 magazine 字符串，哈希表中对应的字符增加。
- 遍历 ransomNote 字符串，将哈希表中对应的字符减一，如果在减一之前该位置的值为 0，则说明 magazine 中没有足够的字符构建 ransomNote，直接返回 False。
- 遍历结束，返回 True。



# 图解

在这以 ransomNote = "anagram", magazine = "nagaram" 为例。

首先初始化哈希表，根据哈希函数，字母 a ~ z 在哈希表中所占位置如下：

<div align=center>

![d456224f344fe2dca7ab8a29f752a15](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_213438233_0.jpg)

</div>

第一步，遍历字符串 magazine，碰到相应的字符，对应下标的哈希值 + 1。

对于 magazine = "nagaram" ，第 1 个字符为 n，n 对应哈希表的下标为 13，则下标 13 对应值 +1：

<div align=center>

![0e33920d572559e8398dba503203027](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_213500283_0.jpg)

</div>

```Python
# 记录 magazine 中各字符出现的次数
for i in magazine:
    hash[ord(i) - ord('a')] += 1
```

依次遍历完字符串 magazine，哈希表如下图：

<div align=center>

![ac33cbb3b24934991cc911a6e369a4e](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_213534566_0.jpg)

</div>

第二步，遍历字符串 ransomNote，将哈希表中对应的字符减一，如果在减一之前该位置的值为 0，则说明 magazine 中没有足够的字符构建 ransomNote，直接返回 False。

对于 ransomNote = "anagram"，第 1 个字符为 a，a 对应哈希表下标为 0，则下标 0 对应值 -1：

<div align=center>

![1b810c9a2ea40fb8003ff494c3b9323](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_213555571_0.jpg)

</div>

```Python
for i in ransomNote:
    # 遍历 ransomNote，如果当前字符的 hash 为 0，证明 magazine 不能组成 ransomNote
    if hash[ord(i) - ord('a')] == 0:
        return False
    # 否则，对 hash 中对应的字符做减操作
    else:
        hash[ord(i) - ord('a')] -= 1
```

同样，依次遍历完字符串 ransomNote，哈希表如下图：

<div align=center>

![6d058e99cc995942d79ab81a2a01379](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_213639823_0.jpg)

</div>

第三步，遍历完，直接返回 True。

此题解法，**时间复杂度为 O(n)。**

因为额外开了一个长为 26 的数组，所以**空间复杂度为 O(m)，m = 26**。



# 代码实现



## Python 代码实现

```Python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # 初始化哈希函数，字符串只包含小写字母，故初始化 26 个
        hash = [0] * 26

        # 记录 magazine 中各字符出现的次数
        for i in magazine:
            hash[ord(i) - ord('a')] += 1

        for i in ransomNote:
            # 遍历 ransomNote，如果当前字符的 hash 为 0，证明 magazine 不能组成 ransomNote
            if hash[ord(i) - ord('a')] == 0:
                return False
            # 否则，对 hash 中对应的字符做减操作
            else:
                hash[ord(i) - ord('a')] -= 1

        return True
```



## Java 代码实现

```Java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] arr = new int[26];
        int temp;
        for (int i = 0; i < magazine.length(); i++) {
            temp = magazine.charAt(i) - 'a';
            arr[temp]++;
        }
        for (int i = 0; i < ransomNote.length(); i++) {
            temp = ransomNote.charAt(i) - 'a';
            //对于金信中的每一个字符都在数组中查找
            //找到相应位减一，否则找不到返回false
            if (arr[temp] > 0) {
                arr[temp]--;
            } else {
                return false;
            }
        }
        return true;
    }
}
```


