**螺旋矩阵Ⅱ**，模拟法解决数组的练手好题，面试常见。

这道题没有算法上的难度，考察的是小婊贝们的代码能力和细心水平。

<div align=center>

![5486ff5bdbafd6547fa02f75f5d2a8b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_190824517_0.jpg)

</div>



# LeetCode 59：螺旋矩阵Ⅱ

题目链接：[螺旋矩阵Ⅱ](https://leetcode-cn.com/problems/spiral-matrix-ii/)



## 题意

给定正整数 n，生成一个包含 1 ~ n² 所有元素，且元素按顺时针螺旋排列的 n \* n 正方形矩阵 matrix。



## 示例

输入：n = 3

输出：[[1,2,3],[8,9,4],[7,6,5]]

解释：

<div align=center>

![5c0b88699ffc4a0f0c8ba9031e1ebf8](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_190922221_0.jpg)

</div>



## 提示

- 1 <= n <= 20



# 题目解析


一道**模拟题**，难度中等，面试出现频率极高。

模拟题就是本身不涉及算法，就是单纯根据题目所描述的模拟整个过程从而得到最后的结果。

**这类题天然的考察你的码力，即对编程语言的掌握能力**，一不小心就会各种 bug。

**做这类模拟题的要点就是多在纸上画一下**，别空想把自己想晕了，代码写干净些，方面后面 debug... 

<div align=center>

![a53b285f8372669f03325e7c8202107](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191036733_0.jpg)

</div>

这道题是按“顺时针”顺序对矩阵进行填充，方向无非就是“上下左右”，顺时针的话，**填充就是按照“上->右->下->左”**，写具体点就是：

- 上：从左到右填充。
- 右：从上到下填充。
- 下：从右到左填充。
- 左：从下到上填充。

同时**找好每次填充的边界**，比如最开始的时候最左侧边界 left = 0，最右侧边界 right = n-1，最上侧边界 up = 0，最下侧边界 down = n-1。

此处你应该拿出笔和纸，开始在纸上写写画画了。

理解了上面话，我们来看图解。



# 图解

以 n = 4 为例。

首先初始化结果矩阵和上下左右边界。

<div align=center>

![0780a2766b25720da23716c42927e53](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191109062_0.jpg)

</div>

```Python
# 初始化结果矩阵
res = [[0] * n for i in range(n)]
# 维护当前值
cnt = 1
# 初始化左、右、上、下边界。
left, right, up, down = 0, n-1, 0, n-1
```

接下来按照顺时针顺序填充结果矩阵。

第 1 步最上行从左到右填充：

此时 up = 0，固定不变，从左至右在 [left, right] 间填充。

<div align=center>

![6c16989609fb54279448a9cf8e43540](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191142938_0.jpg)

</div>

```Python
# 从左到右
for i in range(left, right + 1):
    res[up][i] = cnt
    cnt += 1
```

此时最上行被填充完毕，上边界 top 发生了改变，需要向下移动。

<div align=center>

![37e4c4cfeaafba0b02f1f30e9e398cf](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191227783_0.jpg)

</div>

```Python
up += 1
```

第 2 步最右列从上向下填充：

此时 right = n - 1 =3，固定不变，从上至下在 [up, down] 之间填充。

<div align=center>

![5f46c90dc11ca9e8561c16d0a24f674](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191259763_0.jpg)

</div>

```Python
# 从上到下
for i in range(up, down + 1):
    res[i][right] = cnt
    cnt += 1
```

此时最右列被填充完毕，右边界 right 发生了改变，需要向左移动。

<div align=center>

![1c2a2be2bb6edd1b54472c87a74c8ae](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191333988_0.jpg)

</div>

```Python
right -= 1
```

第 3 步最下行从右向左填充：

此时 down = n - 1 = 3 固定不变，从右至左在 [left, right] 之间填充。

<div align=center>

![e62d458df8c4f654adb023129373b0f](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191402722_0.jpg)

</div>

```Python
# 从右到左
for i in range(right, left-1, -1):
    res [down][i] = cnt
    cnt += 1
```

此时最下行被填充完毕，下边界 down 发生了改变，需要向上移动。

<div align=center>

![9fc4216a24a95a54d895c0b362cedd6](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191444057_0.jpg)

</div>

```Python
down -= 1
```

第 4 步最左列从下向上填充：

此时 left = 0 固定不变，从下至上在 [up, down] 之间填充。

<div align=center>

![db9278acf2030a0496fee8cb6a5d683](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191515971_0.jpg)

</div>

```Python
# 从下到上
for i in range(down, up-1, -1):
    res[i][left] = cnt
    cnt += 1
```

此时最左列被填充完毕，左边界 left 发生了改变，需要向右移动。

<div align=center>

![d14ff8a236ffcb7f6640f9eba0beabf](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202112/20211217_191549973_0.jpg)

</div>



```Python
left += 1
```

剩下的步骤就是重复上面 4 步，直到 n² 的数都填完。

本题模拟法矩阵大小为 n²，需要全部遍历且填充，所以**时间复杂度为 O(n²)**。

此外额外维护了一个大小为 n² 的结果矩阵，所以**空间复杂度为 O(n²)**。



# 代码实现



## Python 代码实现

```Python
class Solution(object):
    def generateMatrix(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """
        # 初始化结果矩阵
        res = [[0] * n for i in range(n)]
        # 维护当前值
        cnt = 1
        # 初始化左、右、上、下边界。
        left, right, up, down = 0, n-1, 0, n-1

        while cnt <= n * n:
            # 从左到右
            for i in range(left, right + 1):
                res[up][i] = cnt
                cnt += 1
            up += 1
            # 从上到下
            for i in range(up, down + 1):
                res[i][right] = cnt
                cnt += 1
            right -= 1
            # 从右到左
            for i in range(right, left-1, -1):
                res [down][i] = cnt
                cnt += 1
            down -= 1
            # 从下到上
            for i in range(down, up-1, -1):
                res[i][left] = cnt
                cnt += 1
            left += 1

        return res
```



## Java 代码实现

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int left = 0, right = n - 1, up = 0, down = n - 1;
        int[][] res = new int[n][n];
        int cnt = 1;
        while(cnt <= n * n){
            for(int i = left; i <= right; i++) res[up][i] = cnt++;
            up++;
            for(int i = up; i <= down; i++) res[i][right] = cnt++;
            right--;
            for(int i = right; i >= left; i--) res[down][i] = cnt++; 
            down--;
            for(int i = down; i >= up; i--) res[i][left] = cnt++; 
            left++;
        }
        return res;
    }
}
```



---

**图解螺旋矩阵Ⅱ**到这就结束辣，是不是模拟法搞的焦头烂额了？

没事，刚接触这样的题都是写一堆 bug，做多了，码力上升了就 ok 了。

多点耐心，多写写画画，一步一步的来。

