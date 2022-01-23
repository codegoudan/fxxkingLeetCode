大家好呀，本帅蛋又来啦！

之前我带大家玩转的数组、单链表、栈和队列都是线性结构，实际情况是除了线性结构还有很多的非线性结构存在。

今天来讲的**二叉树**，就是**非线性结构的典型代表**，这种数据结构比又硬又直的线性数据结构复杂的多：概念多、内容多、代码多、屁事多...

<div align=center>

![d69ea326508a194bf18b8d4fdce2832](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_090522000_0.jpg)

</div>

是不是吓住了？不慌，不慌，有蛋在，不迷茫。

接下来我会一点点儿的掰碎了嚼烂了讲，保证把小婊贝们伺候的舒舒服服的。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_090541990_0.jpg" alt="f25661772a8b907e268e826b9abc4ee" style="zoom: 67%;" />

</div>

蛋话不多说，小葵蛋课堂开课了！

<div align=center>

![b2007d8f9d0b1d57f2d761dfaefdbbc](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_090558241_0.jpg)

</div>



# 树

在讲二叉树之前，肯定要先认识下它祖宗，**树，英文名叫 Tree**，长下面这样：

<div align=center>

![aa4cacfe583077a59cfe255e9778229](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_090624759_0.jpg)

</div>



## 节点类型

树中的每个元素叫做**节点**，就是图中你看到的一个个圆圈。

树里的节点类型有点多，看着上面这张图，从上到下，我把几个概念讲一下：



### 父节点 / 子节点

谁指向谁，指向的就是**父节点**，被指向的是**子节点**。

你看上图里面，A 就可以对 B 说“我是嫩爹”，B 就只能对 A 说“好的，爸爸”。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_090717850_0.jpg" alt="23774181b0f41202c705335e22b82cf" style="zoom:67%;" />

</div>



### 兄弟节点

一个节点的所有子节点之间互相称为**兄弟节点**。

上图中节点 B、C 是 A 的子节点，节点 B 和 C 就互称为兄弟节点。



### 根节点 / 叶节点 / 内部节点

没有父节点的就是**根节点**，在一棵非空树中，有且只有一个根节点。

上图中的根节点就是 A。

没有子节点的就是**叶节点**，G、H、I、E、J 都是叶节点。

除去根节点和叶节点之外的其它节点就是**内部节点**，比如 B、C、D、F。



### 祖宗节点

**祖宗结点**是从根节点到该节点之前所经过的所有节点。

上图中节点 H 的祖宗节点就是 D、B、A。



### 子孙节点

某个节点下直至叶子节点的所有节点，都是此节点的**子孙节点**。

上图中节点 B 的子孙节点是 D、E、G、H、I。



## 子树类型

**以某个节点为根节点的树称为该节点的子树。**

下图中“以 B 为根节点的树”为“根节点 A”的子树。

<div align=center>

![785895cd571b8effc9e2a4c2f9c69a6](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_090901874_0.jpg)

</div>

当然 D、G、H、I 组成的树是 B 为节点的子树，F、J 组成的树是 C 为节点的子树。

需要注意的是，**各个子树一定是互不相交的**，像下面这些就不是子树，同样也不是树。

<div align=center>

![6b230bd0763a871fadf771a881b648b](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_090920170_0.jpg)

</div>

你看上面这 E 和 F 相交了，这就不是树。



## 树的其它重要概念

除了上面说的，树里面还有 3 个重要的概念，这 3 个概念有点相似，容易记混：层次、高度、深度。

下面我用一张图来解释这个概念。

<div align=center>

![a6a17625afa2f6142be843714c47f43](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091011048_0.jpg)

</div>



### 层次

**节点的层次**是从根节点开始算起。

根节点是第一层，根节点的子节点层是第二层，依次往下类推。



### 深度

**深度**的话也是从根节点开始算起，依次往下是深度 1、2、...

**最大深度为最大层数数。**

你可以理解成看一口井，从上往下看。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091049715_0.jpg" alt="6c6ce834da2f8ded3cb4d0a8964daba" style="zoom:67%;" />

</div>



### 高度

**高度**和深度正好相反，高度是从下往上。

叶子节点的高度是 1，叶子节点的父节点层高度是 2，依次往上类推。

你可以理解成看高楼，从下往上看。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091117435_0.jpg" alt="1efb0c69495ddb09617d2cabc39b1a8" style="zoom: 50%;" />

</div>



# 二叉树

二叉树，表面上看上去是有“两个叉的树”，这样应该不准确，这上面还得加个限制条件：最多。

> 每个节点最多只有两个叉的树叫二叉树

**既然是最多，那对于每个节点来说，可以一个叉，也可以没有叉**，所以像下图这些都是二叉树。

<div align=center>

![ba25a0cebd68faa83df8be37c9290d4](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091502638_0.jpg)

</div>

上面 5 种包括了二叉树的所有基本形态。



## 二叉树的种类

除了上面讲的基本形态，还有三种特殊的二叉树：满二叉树、完全二叉树、二叉搜索树。

这也是后面我们要经常打交道的二叉树。



### 满二叉树

**满二叉树**就是叶子节点全在最底层，除了叶子节点以外的每个节点都有两个叉。

<div align=center>

![a8977744959102469b5bfeb371b3183](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091536582_0.jpg)

</div>

**如果深度为 k，那满二叉树的总节点数就是 2^k - 1。**

**在同等深度的二叉树中，满二叉树是节点个数最多的。**



### 完全二叉树

**完全二叉树**比较难理解，它的概念其实分前后两部分：

- **除了最底层以外，其余的每一层节点数都是满的**

这个其实比较好理解，就是去掉最后一层，剩下的是一棵满二叉树。

<div align=center>

![47780d5febe08b6c8ad209253e38371](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091618947_0.jpg)

</div>

- **最底层的节点全集中在该层最左边的位置。**

难理解的也是这句话，最后一层如果是第 k 层，那么这一层最少有 1 个叶子节点，最多有 2^(k-1) 个节点，而这些节点都是从左到右依次排列。

<div align=center>

![78f09478d36aa4eed970b1f427c283c](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091638735_0.jpg)

</div>

上面右图不是完全二叉树，因为最后一层没有按照从左到右排列。节点 J 本应该在虚框的位置，但是跳过了。

明白了这两点，完全二叉树就不会搞到你。

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091659808_0.jpg" alt="34b87dba190c87158de4ba4d28a662e" style="zoom:50%;" />

</div>



### 二叉搜索树

**二叉搜索树，又叫二叉排序树或二叉查找树。**

见名知意，从它的别名可以看出，二叉搜索树是有数值的树（不然怎么排序呢），且有序（不然干嘛排序呢）。

既然有数且有序，肯定有合乎这俩的性质。

**二叉搜索树有以下的性质**：

- 若左子树不空，那左子树所有节点的值均 < 根节点的值。
- 若右子树不空，那右子树所有节点的值均 > 根节点的值。
- 左右子树也均为二叉搜索树。

<div align=center>

![b05ee63af8567d4258b027b26329365](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091737958_0.jpg)

</div>



## 二叉树的存储方式

二叉树具有两种存储方式：顺序存储和链式存储。



### 顺序存储

二叉树**顺序存储**其实就是用数组存，就做稍微了解即可，一般不用。

<div align=center>

![7095246bf3eb39cec22704065e10c90](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091811920_0.jpg)

</div>

其中虚线为不存在的节点，设置为 ^ 表示。



### 链式存储

二叉树最多有两个叉，左孩子和右孩子，那**对于每个节点来说，链式存储需要一个数据域 + 两个指针域。**

数据域存储节点的数值，两个指针域一个指向左孩子，一个指向右孩子。

<div align=center>

![e6dd44646c6acc671eef22bd7f1182d](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091906379_0.jpg)

</div>

节点定义方式 Python 代码如下所示：

```Python
class BTNode:
    def __init__(self,data):
        self.data = data
        self.lchild = None
        self.rchild = None
```

<div align=center>

![66e5f19add553b62a2e32a64a3c4ac3](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_091931591_0.jpg)

</div>



## 二叉树的遍历

**二叉树的遍历就是从根节点开始，按照某种顺序依次访问二叉树中的节点。**

主要有四种遍历方式：

- 前序遍历
- 中序遍历
- 后续遍历
- 层次遍历

在这里，我只先讲基础的概念，因为对于遍历来说，存在不同的遍历方法，内容比较多。

二叉树的遍历是面试中非常非常高频的面试题，关于具体的实现我会单独拿一篇文章来讲。



### 前中后序遍历

一棵二叉树来说，可以将其粗略的分为 3 部分：根节点、左子树和右子树。

而前中后序遍历是对这三部分的不同遍历顺序：

- **前序遍历**：根左右
- **中序遍历**：左根右
- **后序遍历**：左右根

当然对于左子树或者右子树来说，也是如上的遍历方式。

在这我以 preOrder 表示前序遍历，以 inOrder 表示中序遍历，以 postOrder 表示后续遍历，则三者的递推公式为：

- 前序：root -> preOrder(root.left) -> preOrder(root.right)。
- 中序：inOrder(root.left) -> root -> inOrder(root.right)。
- 后续：postOrder(root.left) -> postOrder(root->right) -> root。

我们来看一个例子。

<div align=center>

![2b538295a43edff814b661e7f482dcf](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_092059008_0.jpg)

</div>

对于上图的二叉树：

前序遍历，每一部分按照“根左右”，遍历顺序为 ABDHIEJCFG。

中序遍历，每一部分按照“左根右”，遍历顺序为 HDIBEJAFCG。

后续遍历，每一部分按照“左右根”，遍历顺序为 HIDJEBFGCA。

相信你也做对了。



### 层次遍历

**层次遍历就是表面意思，一层层的遍历，同一层的遍历按照从左到右逐个遍历。**

还是这个图：

<div align=center>

![bdf052fdc46c04526161db5762212f7](https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_092128857_0.jpg)

</div>

层次遍历的顺序为：ABCDEFGHIJ。



---

好了，到这**二叉树的入门基础**就讲完了。

基本上关于树中涉及的概念和二叉树中种类、存储方式以及遍历方式做了简单明了且通俗易懂的讲解。

基本上重要的内容都在这了，只要能认认真真看到这的，肯定已经对二叉树有了一个大概的认识。

**看完就会，在数据结构与算法的学习不是说说而已。**

<div align=center>

<img src="https://gitee.com/codegoudan/codegoudanIMG/raw/master/202201/20220123_092224557_0.jpg" alt="850d9fd2c5cd251b3150d0eb71edb0e" style="zoom:50%;" />

</div>