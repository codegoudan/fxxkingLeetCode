二叉树已经带着大家玩了好长一段时间了，今天呢，我想来给大家重点讲一下二叉搜索树。

关于二叉搜索树，之前只是在【[**二叉树入门**](https://mp.weixin.qq.com/s/l8jwYfaUuV5ZjFH8CMNw9A)】文章中简单提及了一下，并没有深入去讲。

今天呢，咱就把架势拉开，好好的看一下二叉搜索树的真面目~

<div align=center>

![ecsss-14](https://cdn.codegoudan.com/img/ecsss-14.jpg)

</div>

废话不多说，小葵蛋课堂开课了！

<div align=center>

![ecsss-0](https://cdn.codegoudan.com/img/ecsss-0.png)

</div>



# 二叉搜索树

要学二叉搜索树，那首先什么是二叉搜索树？

**二叉搜索树，又叫二叉排序树或二叉查找树。**

见名知意，二叉搜索树首先是棵二叉树，从它的别名可以看出，二叉搜索树是有数值的树（不然怎么排序呢），且有序（不然干嘛排序呢）。

既然有数且有序，肯定有合乎这俩的性质。

**二叉搜索树有以下的性质**：

- 若左子树不空，那左子树所有节点的值均 < 根节点的值。
- 若右子树不空，那右子树所有节点的值均 > 根节点的值。
- 左右子树也均为二叉搜索树。

<div align=center>

![ecsss-1](https://cdn.codegoudan.com/img/ecsss-1.png)

</div>

仔细观察上面的二叉搜索树，你会发现当**对二叉搜索树进行中序遍历时，得到的结果是一个有序的序列**，比如最左侧二叉树遍历结果是：

<div align=center>

![ecsss-2](https://cdn.codegoudan.com/img/ecsss-2.png)

</div>

知道了概念，那二叉搜索树有什么用处呢？

其实**二叉搜索树为了搜索（查找）而生**，但它又不仅仅是为了搜索（查找）。

<div align=center>

![ecsss-15](https://cdn.codegoudan.com/img/ecsss-15.jpg)

</div>

还有另外的目的，那就是**提高【插入数据】和【删除数据】的速度**。

那下面就来看一下二叉搜索树的几个操作是怎么整的。



# 二叉搜索树操作

二叉搜索树的操作主要有 3 种：

- 查找操作
- 插入操作
- 删除操作

下面我们就来看下这 3 个操作是怎么实现的。



## 查找操作

根据二叉搜索树的定义，在**二****叉搜索树中查找一个节点**，其实就 3 步：

- 将查找的节点根节点比较，如果相等，则直接返回。
- 如果查找的节点 < 根节点，则在左子树中递归查找。
- 如果查找的节点 > 根节点，则在右子树中递归查找。

比如对于下图：

<div align=center>

![ecsss-3](https://cdn.codegoudan.com/img/ecsss-3.png)

</div>

我们此时要查找 node = 31 的节点。

第 1 步：与根节点比较，根节点的值为 29，node > 29，所以去右子树中继续查找。

<div align=center>

![ecsss-4](https://cdn.codegoudan.com/img/ecsss-4.png)

</div>

第 2 步：此时右子树的根节点值为 30，node > 30，则继续在右子树中查找。

<div align=center>

![ecsss-5](https://cdn.codegoudan.com/img/ecsss-5.png)

</div>

第 3 步：当前右子树的根节点值为 31，与 node 相等，直接返回。

<div align=center>

![ecsss-6](https://cdn.codegoudan.com/img/ecsss-6.png)

</div>



## 插入操作

**二叉搜素树的插入操作，其实和二叉树的插入一样，将要插入的节点 node 放在树中合适的位置。**

这个“寻找合适位置”的过程和二叉搜索树的查找操作类似，只不过多了一步“插入”，而且**对于新插入的节点来说，这个“位置”一般都是在叶子节点上**。

具体的步骤就是：

- 如果插入的节点值比根节点的数值小：

- - 如果根节点的左子树为空，那该节点直接插入到根节点的左孩子位置。
  - 如果根节点的左子树不为空，继续遍历左子树寻找插入的位置。

- 如果插入的节点值比根节点的数值大：

- - 如果根节点的右子树为空，那该节点直接插入到根节点的右孩子位置。
  - 如果根节点的右子树不为空，继续遍历右子树寻找插入的位置。

比如对于下图：

<div align=center>

![ecsss-3](https://cdn.codegoudan.com/img/ecsss-3.png)

</div>

我们此时要插入 node = 3 的节点。

第 1 步：根节点的值为 29，node < 29，且节点的左孩子不为空，继续遍历左子树寻找插入的位置。

<div align=center>

![ecsss-7](https://cdn.codegoudan.com/img/ecsss-7.png)

</div>

第 2 步：左子树的根节点值为 4，node < 4，且节点的左孩子不为空，继续遍历左子树寻找插入的位置。

<div align=center>

![ecsss-8](https://cdn.codegoudan.com/img/ecsss-8.png)

</div>

第 3 步：左子树的根节点值为 0，node > 0，且节点的右孩子为空，插入该节点右孩子的位置。

<div align=center>

![ecsss-9](https://cdn.codegoudan.com/img/ecsss-9.png)

</div>



## 删除操作

删除操作是二叉搜索树相对较麻烦的一个操作，因为我们在删除节点之后仍然要保持剩下的二叉树仍是二叉搜索树，即依然满足二叉搜索树的特性。

你不能一刀下去，李逵变李鬼。

<div align=center>

![ecsss-16](https://cdn.codegoudan.com/img/ecsss-16.jpg)

</div>

**二叉搜索树的删除操作涉及的情况比较多，主要是三种**，我们一一来看。



**情况 1：删除节点为二叉树的叶子节点。**

这种情况很好处理，**通过二叉搜索树的查找操作找到节点，然后直接删掉就好了**。

删掉叶子节点对二叉树的其它节点没有什么影响。

比如对于下图：

<div align=center>

![ecsss-3](https://cdn.codegoudan.com/img/ecsss-3.png)

</div>

我们此时要删除 node = 0 的节点，查找到该节点，然后直接删除。

<div align=center>

![ecsss-10](https://cdn.codegoudan.com/img/ecsss-10.png)

</div>



**情况 2 ：删除的节点只有左子树或者右子树。**

碰到这种情况也比较好解决，那就是**直接用删除节点的父节点指向它的子树即可**。

比如对于下图：

<div align=center>

![ecsss-3](https://cdn.codegoudan.com/img/ecsss-3.png)

</div>

我们此时要删除 node = 30 的节点，查找到该节点，然后删除后，二叉搜索树的变化如下所示：

<div align=center>

![ecsss-11](https://cdn.codegoudan.com/img/ecsss-11.png)

</div>



**情况 3：删除的节点既有左子树又有右子树。**

这个就稍微复杂点了，过程我就略过了，也不是很重要，直接说答案：

就是**将要删除节点 node 位置上替换成左子树最右边的节点或者右子树最左边的节点。**

**即左子树的最大值或者右子树的最小值。**

比如对于下图：

<div align=center>

![ecsss-3](https://cdn.codegoudan.com/img/ecsss-3.png)

</div>

我们此时要删除 node = 4 的节点，它的左子树的最大值为 0，右子树的最小值为 5。

所以删除完 node = 4 后，二叉搜索树可以变成下面这样：

<div align=center>

![ecsss-12](https://cdn.codegoudan.com/img/ecsss-12.png)

![ecsss-13](https://cdn.codegoudan.com/img/ecsss-13.png)

</div>

---

好啦，**二叉搜索树的入门基础**到这就讲完了。

主要的内容其实就是在二叉搜索树的性质和操作做了简单明了且通俗易懂的讲解。

基本上重要的内容都在这了，只要能认认真真看到这的，肯定已经对二叉树搜索树有了直观的认识。

下面就是在实战中来体味二叉搜索树的魅力了~
