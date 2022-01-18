<div align=center>

![c03e053bbca528b12bfaaa267bf0378](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_215143253_0.jpg)

</div>



# LeetCode 202：快乐数

题目链接[快乐数](https://leetcode-cn.com/problems/happy-number/)



## 题意

判断正整数 n 是不是快乐数。

快乐数定义：

（1）每次将正整数替换为它每个位置上的数字的平方和。

（2）重复这个过程直到这个数变为 1，也可能是无限循环但始终变不到 1。

（3）如果可以变为 1，这个数就是快乐数。



## 示例

输入：19

输出：true

解释：

1² + 9² = 82

8² + 2² = 68

6² + 8² = 100

1² + 0² + 0² = 1



## 提示

- 1 <= n <= 2^31 -1



# 题目解析

这道快乐数，把它归为难度简单的一类题，我感觉更多的是代码层面上。

实际上这道题，乍一看很具有迷惑性，看“示例”的时候很容易把它当作一道数学题。

当你一不小心陷入这个数学题的怪圈之后，基本就走进了死胡同。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_215359676_0.jpg" alt="4397978ad8484a3a1ca3af63213a259" style="zoom:67%;" />

</div>

所以**读题细致，也是大家在平时刷题要注意的**。

这道题我是怎么知道可以用哈希解决呢？重点是在“**无限循环**”上。

什么叫循环，循环就是出现了一遍又一遍，如果循环了那肯定就不是快乐数。

那这道题就可以转化成：【**在“将正整数替换为它每个位置上的数字的平方和”过程中，新出现的正整数是否曾经出现过。**】

你看，现在就很明了了，我们记录出现过的正整数，然后将新元素与之前出现过的正整数比较。

**在一堆数中查找一个数，当然是祭出哈希**，毕竟是查找中的秒男，时间复杂度为 O(1)。

不过这里和之前做的【[字母异位词](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475921505&idx=1&sn=edf8b3420e0b1bf406f98a990074bbe0&chksm=ff22f7ecc8557efa3edfac0d6f107b67bffca87d5debb474e1d33b7b12a7c73ba3b756a280d1&scene=21#wechat_redirect)】还不太一样，字母异位词是有限的，只有 26 个小写字母，建一个长度为 26 的数组即可。

快乐数则不行，因为对目前的我们来说不清楚会产生多少个不同的数，也不清楚每个数的值是多少。

**碰到这种对目前来说是未知数量和未知数值大小的情况，我们可以使用集合 set 来解决。**

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_215452403_0.jpg" alt="82fab570e2ffff1afd6a7c85e20f98d" style="zoom:50%;" />

</div>

想明白了这个，那快乐数的求解每次就可以光明正大的分为两步：

- 将当前数进行**数位分离**，求各位上平方的和。
- 每次生成的数，**查是否在哈希集合中**，在的话就不是快乐数，不在的话就添到集合里。



# 图解

以 n = 19 为例。

首先说一下如何进行数位分离，很简单：

每次用 n 对 10 取余（19 % 10 = 9），再求平方（happy_sum = 9 * 9 = 81），最后对 10 整除（n = 19 / 10 = 1）。

```Python
# 求正整数 num 每个位置上数字的平方和
def getNext(self, num):
    happy_sum = 0
    while num:
        happy_sum += (num % 10) ** 2
        num = num // 10
    return happy_sum
```

下面就正式开始图解。

首先初始化一个集合，记录过程数据。

```Python
# 记录过程数据
mid = set()
```

第 1 步，n = 19，数位分离求平方和 1² + 9² = 82，82 不在集合中，遂添加进集合。

<div align=center>

![aa3cc3b99bbbd7b48c18ddd18bca28c](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_215616411_0.jpg)

</div>

```Python
# 如果替换后的数再之前出现过，则说明陷入无限循环，此数不是快乐数
if n in mid:
    return False
else:
    mid.add(n)
```

第 2 步，n = 82，数位分离求平方和 8² + 2² = 68，68 不在集合中，遂添加进集合。

<div align=center>

![25f2276116c809c2dd8632d664a2c5c](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_215646689_0.jpg)

</div>

第 3 步，n = 68，数位分离求平方和 6² + 8² = 100，100 不在集合中，同样添加进集合。

<div align=center>

![4011a555c543172f6b2ccc2cc766d66](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_215706824_0.jpg)

</div>

第 4 步，n = 100，数位分离求平方和为 1，返回 True，所以最初的 n = 19 为快乐数。

```Python
# 如果为1，则是快乐数
if n == 1:
    return True
```

可能还会有小婊贝不清楚为啥曾经出现过就肯定不是快乐数，直接举个例子吧。

以 n = 2 为例吧，我省去中间步骤，直接看下图。

<div align=center>

![19d75a35ae7ece6cf81b7f885cf5783](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220102_215738110_0.jpg)

</div>

这道题的时间复杂度由两部分组成：一是求每个位置上数字的平方和为 O(logn)，二是看新元素是否在集合中，单个查询的时间复杂度为 O(1)，因为总会是在有限的个数内解决掉这个问题，所以总的**时间复杂度为 O(logn)**。

因为额外建了一个集合 mid，所以**空间复杂度为 O(logn)**。



# 代码实现



## Python 代码实现

```Python
class Solution:

    # 求正整数 num 每个位置上数字的平方和
    def getNext(self, num):
        happy_sum = 0

        while num:
            happy_sum += (num % 10) ** 2
            num = num // 10

        return happy_sum

    def isHappy(self, n: int) -> bool:

        # 记录过程数据
        mid = set()

        while True:
            # 将当前数替换为它每个位置上的数字的平方和。
            n = self.getNext(n)
            # 如果为1，则是快乐数
            if n == 1:
                return True
            # 如果替换后的数再之前出现过，则说明陷入无限循环，此数不是快乐数
            if n in mid:
                return False
            else:
                mid.add(n)
```



## Java 代码实现

```Java
class Solution {
    private int getNextNumber(int n) {
        int res = 0;
        while (n > 0) {
            int temp = n % 10;
            res += temp * temp;
            n = n / 10;
        }
        return res;
    }    
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextNumber(n);
        }
        return n == 1;
    }
}
```



---

**图解快乐数**到这就结束辣，是不是快乐的知识增加啦？

这又是一种新的哈希应用的题目，和之前的有所区别，一个是有限，一个是无限。

还是那句话，要仔细揣摩，掌握不同类型题的解决办法。
