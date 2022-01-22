**在排序数组中查找元素的第一个和最后一个位置**，这道题和我们之前做的二分查找又有些区别。

这次要查找的数组虽然也是升序的整数数组，但是它**存在重复的元素**。

<div align=center>

![645a501b13c2e3dacf13234c41a4fe2](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163221754_0.jpg)

</div>



# LeetCode 34：排序数组首尾元素位置

题目链接：[在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)



## 题意

给定一个按照升序排列的整数数组 nums 和一个目标值 target，找出给定目标值在数组中的开始位置和结束位置。

如果 target 不在数组中，返回 [-1,-1]。



## 示例

输入：nums = [5,7,7,8,8,10], target = 8

输出：[3,4]



## 提示

- 0 <= len(nums) <= 10^5
- -10^9 <= nums[i] <= 10^9
- nums 是一个非递减数组
- -10^9 <= target <= 10^9



# 题目解析

难度中等，二分查找经典好题。

到目前为止，我们已经做了两道二分查找的题：

[ACM 选手图解 LeetCode 算术平方根](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475922861&idx=1&sn=1cc6714237eefb1c283c80404c2d6020&chksm=ff22f220c8557b36b534649f9d1d1b73636cfa7d9f03a2c23ab108b6dfd29d674c02dd7ca8bd&scene=21#wechat_redirect)这道题：**整数数组 nums 有序、无重复、target 能在数组中找到**。

[ACM 选手图解 LeetCode 搜索插入位置](http://mp.weixin.qq.com/s?__biz=MzI0NjAxMDU5NA==&mid=2475922963&idx=1&sn=f3924559f5f7ea65ea1df77958fe8fe6&chksm=ff22f19ec85578887e38abd94ed5aecf2e4bbd1428806ef39dc323bb365e525a08b7bfd2d48e&scene=21#wechat_redirect)这道题：**整数数组 nums 有序、无重复、target 不一定能在数组中找到**。

而本题则是：**整数数组 nums 有序、有重复、target 不一定能在数组中找到**。

nums 存在重复的元素，这就意味着在 nums 数组中二分查找 target 返回的下标是不唯一的。

但幸运的是 nums 有序。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163449833_0.jpg" alt="66442dd7c886fcbecce858a106fc456" style="zoom:33%;" />

</div>

**就算有重复元素，重复元素也是挨在一起的，挨在一起就存在区间**，本题就是查找 target 在 nums 中的区间，即查找 target 在 nums 中的左右边界。

仔细想的话，要找 target 在 nums 数组中的左右边界，**无非存在 3 种情况**：

- target 在 nums[0] ~ nums[n-1] 中，nums 中存在 target。例如 nums = [5,7,7,8,8,10]，target = 8，返回 [3，4]。
- target 在 nums[0] ~ nums[n-1] 中，nums 中不存在 target。例如 nums = [5,7,7,8,8,10]，target = 6，返回 [-1,-1]。
- target < nums[0] 或者 target > nums[n-1]。例如 nums = [5,7,7,8,8,10], target = 4，返回 [-1,-1]。

为了照顾初学者，这道题我不炫技，用最简单易懂的方式来让大家理解。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163509690_0.jpg" alt="25de7024431039d8942a7b413449b66" style="zoom:50%;" />

</div>

**用两个二分查找，一个二分查找查找左边界，另一个查找右边界**，分情况讨论上述的 3 种情况。

下面来看图解。



# 图解

以 nums = [5,7,7,8,8,10], target = 8 为例。

首先判断 target 的范围：

```Python
if len(nums) == 0 or nums[0] > target or nums[-1] < target:
    return [-1,-1]
```

此例中，target = 8，在 nums[0] ~ nums[n-1] 返回内，下面开始寻找左右边界。



## 第一步：寻找左边界。

1. 初始化 low 和 high 指针。

<div align=center>

![60a47e4e3feef4e02942ee6c5168d92](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163627593_0.jpg)

</div>

```Python
low, high = 0, len(nums) - 1
```

2. low = 0，high = 5，mid = low + (high - low) // 2 = 2：

<div align=center>

![c34598f63e0fa4c475f52ddf4018511](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163705968_0.jpg)

</div>

```Python
mid = low + (high - low) // 2
```

此时 nums[mid] = 7 < target，所以 low 向右移动至 mid + 1 = 3 处：

<div align=center>

![da1f624a9e6cca2f15b74ef1b78fca3](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163732928_0.jpg)

</div>

```Python
if nums[mid] < target:
    low = mid + 1
```

3. low = 3，high = 5，mid = low + (high - low) // 2 = 4：

<div align=center>

![f9ee99dda7deae219ce4ce6cb76e80f](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163807455_0.jpg)

</div>

此时 nums[mid] = 8 == target，所以 high 向左移动至 mid - 1 = 3 处：

<div align=center>

![89722e7bd7ee237738e4a9270453f52](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163821959_0.jpg)

</div>

```Python
if nums[mid] == target:
    high = mid - 1
```

上面的代码是找左边界的精髓所在。

**普通二分查找是，当 nums[mid] == target 时，直接返回 mid，而在本题中，则是要继续向左查找，看是否还有和 target 相等的数组元素**。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163854142_0.jpg" alt="9ae51678347f5dc353c598a1f79beaa" style="zoom: 25%;" />

</div>

4. low = 3，high = 3，mid = low + (high - low) // 2 = 3：

<div align=center>

![2ee41f7d63a41a903efd7d8bd859dff](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163924712_0.jpg)

</div>

此时 nums[mid] = 8 == target，所以 high 向左移动至 mid - 1 = 2 处：

<div align=center>

![b7a0d9aca6d311b69f2acdeabdbcd4b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163937658_0.jpg)

</div>

此时 low > high，while 循环中止，此时的 nums[low] == target，所以左边界为 low = 3。

<div align=center>

![745cf27da00bb182450ad673d637164](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_163953385_0.jpg)

</div>

```Python
if nums[low] == target:
    return low
```



## 第二步：寻找右边界。

1. 初始化 low 和 high 指针。

<div align=center>

![5e4d9a8b57d8079e56548804cf31a12](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164045847_0.jpg)

</div>

```Python
low, high = 0, len(nums) - 1
```

2. low = 0，high = 5，mid = low + (high - low) // 2 = 2：

<div align=center>

![67a6aaaab16977b1805b637ebacdb1c](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164129379_0.jpg)

</div>

```Python
mid = low + (high - low) // 2
```

此时 nums[mid] = 7 < target，所以 low 向右移动至 mid + 1 = 3 处：

<div align=center>

![0d35f494bf07fa312c67cfbc27133b0](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164200969_0.jpg)

</div>

```Python
if nums[mid] < target:
    low = mid + 1
```

3. low = 3，high = 5，mid = low + (high - low) // 2 = 4：

<div align=center>

![d30a59a095001dd8b307c0cd871415b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164237280_0.jpg)

</div>

此时 nums[mid] = 8 == target，所以 low 向右移动至 mid + 1 = 4 处：

<div align=center>

![c1567266f6b9346c1b025fa3cb0fd29](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164253268_0.jpg)

</div>

```Python
if nums[mid] == target:
    low = mid + 1
```

同样，**上面的代码是找右边界的精髓所在，一直向右找，看是否还有和 target 相等的数组元素**。

4. low = 4，high = 4，mid = low + (high - low) // 2 = 4：

<div align=center>

![fe36ae38726d36e2db9571ecdbc609f](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164326487_0.jpg)

</div>

此时 nums[mid] = 8 == target，所以 low 向右移动至 mid + 1 = 5 处：

<div align=center>

![9118e8a738d0014d8d9e0944974b546](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164343670_0.jpg)

</div>

此时 low > high，while 循环中止，此时的 nums[high] == target，所以右边界为 high = 4。

<div align=center>

![439ecd79fdc50082a42f58ce15c4a23](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164358253_0.jpg)

</div>

```Python
if nums[high] == target:
    return high
```

此时左右边界都已找到，至此结束，返回 [3,4]。

<div align=center>

![db87757ae4f791c6f1a2429359b1a63](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164420520_0.jpg)

</div>

本题解使用二分查找，一共执行两次，所以**时间复杂度为 O(logn)**。

除了几个指针外，并无占用其它空间，所以**空间复杂度为 O(1)**。



# 代码实现



## Python 代码实现

```Python
class Solution:

    # 寻找左边界
    def leftMargin(self, nums: List[int], target: int):

        low, high = 0, len(nums) - 1

        while low <= high:
            mid = low + (high - low) // 2

            # 如果 nums[mid] = target，继续向左寻找左边界
            if nums[mid] == target:
                high = mid - 1
            elif nums[mid] > target:
                high = mid - 1
            else:
                low = mid + 1
        if nums[low] == target:
            return low
        # 如果左边界的值不等于 target
        else:
            return -1

    # 寻找右边界
    def rightMargin(self, nums: List[int], target: int):

        low, high = 0, len(nums) - 1

        while low <= high:
            mid = low + (high - low) // 2

            # 如果 nums[mid] = traget，继续向右寻找右边界
            if nums[mid] == target:
                low = mid + 1
            elif nums[mid] > target:
                high = mid - 1
            else:
                low = mid + 1
        if nums[high] == target:
            return high
        # 如果右边界的值不等于 target
        else:
            return -1

    def searchRange(self, nums: List[int], target: int) -> List[int]:

        if len(nums) == 0 or nums[0] > target or nums[-1] < target:
            return [-1,-1]

        lm = self.leftMargin(nums, target)
        rm = self.rightMargin(nums, target)

        return [lm,rm]
```



## Java 代码实现

```Java
class Solution {

    public int leftMargin(int[] nums,int target){
        int low = 0;
        int high = nums.length - 1;
        while(low <= high){
            int mid = low + (high-low)/2;
            if(nums[mid] < target){
                low = mid + 1;
            }else if(nums[mid] > target){
                high = mid - 1;
            }else if(nums[mid] == target){
                high = mid - 1;
            }
        }
        if (nums[low] == target) {
            return low;
        } else {
            return -1;
        }
    }
    public int rightMargin(int[] nums,int target){
        int low = 0;
        int high = nums.length - 1;
        int rm = -2;
        while(low <= high){
            int mid = low + (high-low)/2;
            if(nums[mid] < target){
                low = mid + 1;
            }else if(nums[mid] > target){
                high = mid - 1;
            }else if(nums[mid] == target){
                low = mid + 1;
                rm = low;
            }
        }
        if (nums[high] == target) {
            return high;
        } else {
            return -1;
        }
    }
    
    public int[] searchRange(int[] nums, int target) {
        if (nums.length == 0 || nums[0] > target || nums[nums.length - 1] < target) {
            return new int[] {-1,-1};
        }
        int lm = leftMargin(nums,target);
        int rm = rightMargin(nums,target);
        return new int[] {lm,rm};
    }
}
```



---

**图解元素在排序数组的区间位置**到这就结束辣，自认为写的很清楚了，是不是收获满满？

到目前为止三道二分查找的实战题，解决了三种不同情况的数组，自己要仔细揣摩差别。

二分查找就这些东西，在边界和细节上搞人，只要每次做题小心点，就没啥问题。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220104_164554900_0.jpg" alt="51515c65aae31ec33d17bd53ac08e8b" style="zoom: 50%;" />

</div>