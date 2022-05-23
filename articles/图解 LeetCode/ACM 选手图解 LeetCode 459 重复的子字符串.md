**重复的子字符串**，是 KMP 算法的变形题。

这道题比较有意思，把我卡了俩小时，后来是在健身房推胸的时候灵光一闪...

已经很久没碰到要提交两次才能 AC 的题了。

<div align=center>

![459-0](https://cdn.codegoudan.com/img/459-0.jpg)

</div>

牛皮不多吹，我们来看一下这道题。

<div align=center>

![459-1](https://cdn.codegoudan.com/img/459-1.png)

</div>



# LeetCode 459：重复的子字符串

题目链接：[重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/)



## 题意

给定一个非空的子字符串，判断它是否可以由它的一个子串重复多次构成。



## 示例

输入："abab"
输出：True



## 提示

- 给定的字符串只含有小写英文字母，并且长度不超过 10000。



# 题目解析

官方难度评级，本题为简单，我怀疑 LeetCode 是在搞心态...

这要是道简单题，我直播...咳咳...

<div align=center>

![459-2](https://cdn.codegoudan.com/img/459-2.jpg)

</div>

之前 KMP 算法我们说的是解决模式串在主串中的位置，重复的子字符串这道题不是求子串在主串中出现的位置，而是判断一个字符串是不是由多个重复子串构成。

官方点讲，这是**周期性字符串问题，KMP 是解决这类问题的经典解法**。

但是虽说是 KMP，但是用到的只是求 next 的那一块，剩下的，得靠推公式...

我直接先来说下公式：



```
len(s) % (len(s) -  maxLen) = 0
```



其中 **len(s) 为字符串 s 的长度**，**maxLen 为最长公共前后缀的长度**。

所以这个公式翻译一下就是：**如果 s 是周期串，那【s 的长度】是【s 的长度减去最长公共前后缀的长度】的倍数，那字符串 s 就是周期串**。

至于为什么是这个公式，就得从盘古开天辟地开始了...

<div align=center>

![459-3](https://cdn.codegoudan.com/img/459-3.jpg)

</div>

有兴趣的去仔细研究下这篇文章的推理，讲的真的非常好，图文并茂，保证认真看完就懂了...



> https://writings.sh/post/algorithm-repeated-string-pattern



至于上面所说的最长公共前后缀就是 next。

具体如何求我在之前 KMP 的入门文章中有详细介绍：



[**ACM 选手带你玩转 KMP 算法！**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924907&idx=1&sn=6f6fc1475be2d7d2ca5ab6e0ec755bca&chksm=ff22fa26c8557330a906f6ed9f444d71064a590109b093d8e97f0ab1cd82e5106a5138e8aecd&scene=21#wechat_redirect)

<div align=center>

![459-4](https://cdn.codegoudan.com/img/459-4.png)

</div>

<div align=center>

![459-5](https://cdn.codegoudan.com/img/459-5.png)

</div>

next 数组是**从 0 开始的，且它存的是位置**，所以最长公共前后缀的长度应该是 **next[len(s) - 1] + 1**。

但是只知道公式是不够的，这道题还是会卡住，没错，就是我...

```Python
if maxLen == 0 or s[n - 1] != s[n - 1 - maxLen]:
    return False
```

or 前面那个我就不说了，maxLen == 0 即 next[len(s) - 1] == -1，找无可找，肯定不存在重复。

下面来重点说下 or 后面这个，我就是缺了这行代码。

<div align=center>

![459-6](https://cdn.codegoudan.com/img/459-6.jpg)

</div>

这是一种特殊的情况，按照我在【[**KMP 算法入门**](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475924907&idx=1&sn=6f6fc1475be2d7d2ca5ab6e0ec755bca&chksm=ff22fa26c8557330a906f6ed9f444d71064a590109b093d8e97f0ab1cd82e5106a5138e8aecd&scene=21#wechat_redirect)】文章里讲的 next 求法：

“abab” 的 next 值为 [-1,0,0,1]，“abac” 的 next 值也为 [-1,0,0,1]，前者是周期串，后者不是，但是后者的 next 值和前者一样，都会被算成周期串。

那怎么解决呢？就是加一个限制条件：

```Python
s[n - 1] != s[n - 1 - maxLen]
```

即**如果是周期串，那对应位置上的字母应该是一样的，如果对应位置上的字母不一样，那就肯定不是周期串**。

<div align=center>

![459-7](https://cdn.codegoudan.com/img/459-7.png)

</div>

大家一定要注意。

<div align=center>

![459-8](https://cdn.codegoudan.com/img/459-8.gif)

</div>



# 代码实现



## Python 代码实现

```Python
class Solution:
    def getNext(self, s: str):
        # 后缀匹配指向
        i = 0
        # 前缀匹配指向
        j = -1
        # 初始化 next 数组
        next = [-1] * len(s)

        # 此处 next[0] = -1，所以只需要求剩下的 len(T)-1 个即可
        while i < len(s) - 1:
            # j == -1 就是找无可找 or 匹配成功，相同前缀长度增加1
            if j == -1 or s[i] == s[j]:
                i += 1
                j += 1
                next[i] = j
            # 匹配不成功则在前面的子串中继续搜索，直至找不到（即 j== -1 的情况）
            else:
                j = next[j]

        return next

    def repeatedSubstringPattern(self, s: str) -> bool:

        if len(s) == 0:
            return False

        next = self.getNext(s)
        n = len(s)
        # 最长公共前后缀
        maxLen = next[n - 1] + 1

        if maxLen == 0 or s[n - 1] != s[n - 1 - maxLen]:
            return False

        return n % (n - maxLen) == 0
```



## Java 代码实现

```Java
class Solution {
    
    public int[] getNext(String s){
        int i = 0;
        int j = -1;
        int n = s.length();
        
        int[] next = new int[n];
        next[0] = -1;

        while(i < n - 1){
            if(j == -1 || s.charAt(i) == s.charAt(j)){
                i += 1;
                j += 1;
                next[i] = j;
            }else{
                j = next[j];
            }
        }
        return next;
    }


    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();

        if (n == 0){
            return false;
        }

        int[] next = getNext(s);
        int maxLen = next[n - 1] + 1;

        if(maxLen == 0 || s.charAt(n - 1) != s.charAt(n - 1 - maxLen)){
            return false;
        }

        return n % (n - maxLen) == 0;
    }
}
```



---

**图解重复的子字符串**到这就结束辣，呃，以后碰到这种类型的题记得用 KMP 就好了。

如果能把公式以及为何公式要这么推搞懂更好。

如果搞不懂问题也不大，本身只是为了让你练习一下 KMP 的用法，**不要被困住**。

<div align=center>

![459-9](https://cdn.codegoudan.com/img/459-9.jpg)

</div>