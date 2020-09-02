# 图解：什么是B树？

在学习B树和B+树之前首先考虑一个问题：为什么需要B树、B+树？普通的二叉树或者平衡二叉树不能满足我们的需求吗？

我们知道，B树和B+树一个典型的应用场景是作为数据库索引，下面我们就以这个场景为例，分析一下B树和普通二叉树的区别。

假设我们的数据库存放在磁盘上，有n = `1G` 个记录。

如果我们用平衡二叉树建立索引，那么每次查找需要
$$
\log_210^9 = 30
$$
30次I/O操作，每次I/O只读出一个关键码，得不偿失。

两点事实：

* 不同容量的存储器，访问速度差异悬殊

* 从磁盘中读写`1B`，与读写`1KB`几乎一样快

为了充分利用磁盘对**批量访问**的高效支持，将平衡二叉树中每d代合并为一个超级节点，每下降一层，都以**超级节点**为单位，读入**一组**关键码。所谓m阶B树，即m路平衡搜索树。

如果我们用m=256阶B树建立索引，那么每次查找需要
$$
\log_m10^9 <= 4
$$
每次最多需要4次I/O操作即可。


## 1. 结构

B树相当于把平衡二叉树中每d代合并为一个超级节点

平衡二叉树：

![dXFGAe.png](https://s1.ax1x.com/2020/08/31/dXFGAe.png)

B树：

![dXF37D.png](https://s1.ax1x.com/2020/08/31/dXF37D.png)

从上图我们可以看到，每个B树超级节点中包含3个关键码和4个指向下层节点的指针。

一般地，一个超级节点包含一组关键码和一组指向孩子节点的指针，关键码个数总比孩子指针个数少一个。

![dXFK6x.png](https://s1.ax1x.com/2020/08/31/dXFK6x.png)

m阶B树的约束条件：

* 树根：2 <= 分支数 <= m
* 其余节点：`ceil(m/2)` <= 分支数 <= m

B树的紧凑表示：

![dXFlnK.png](https://s1.ax1x.com/2020/08/31/dXFlnK.png)

**B树索引**：

B树索引的叶节点和非叶节点如下图所示，(a)表示B树索引的叶节点，其中指针Pi指向具有搜索码值Ki的一条文件记录。(b)表示B树索引的非叶节点，其中指针Pi指向孩子节点，Bi指向搜索码Ki对应的一条文件记录。

![dXFMX6.png](https://s1.ax1x.com/2020/08/31/dXFMX6.png)

B树索引的一个示例：

![dXF10O.png](https://s1.ax1x.com/2020/08/31/dXF10O.png)



## 2. 查找

![dXkJbT.png](https://s1.ax1x.com/2020/08/31/dXkJbT.png)

算法：

* 将根节点作为当前节点
* 只要当前节点非外部节点
  * 在当前节点中**顺序查找**
  * 若找到关键码，则
    * 返回**查找成功**
  * 否则
    * 沿指针，转至对应子树
    * 将其根节点**读入**内存
    * 更新当前节点
* 返回查找失败



查找示例：



![dXkvzn.gif](https://s1.ax1x.com/2020/08/31/dXkvzn.gif)

## 3. 插入

算法：

```c++
bool BTree<T>::insert( const T & e ) {
	BTNodePosi(T) v = search( e );
	if ( v ) return false; //确认e不存在
	Rank r = _hot->key.search( e ); //在节点_hot中确定插入位置
	_hot->key.insert( r+1, e ); //将新关键码插至对应的位置
	_hot->child.insert( r+2, NULL ); _size++; //创建一个空子树指针
	solveOverflow( _hot ); //若上溢,则分裂
	return true; //插入成功
}
```

其中`_hot`节点表示上一个查找所访问的超级节点。

算法首先在B树中查找关键码e，确保e不存在(假设B树中各个关键码唯一)。查找必然失败于某个节点`_hot`，然后在该叶节点的适当位置插入关键码和一个空子树指针。

由于B树节点对关键码的最大数目有限制，所以在叶节点插入一个新的关键码可能导致节点中关键码个数上溢，此时需要调用`solveOverflow`函数，将上溢节点分裂。

**分裂**：

设上溢节点中关键码依次为{k0, k1, k2, ..., km-1}

* 取中位数s = m/2， 以关键码ks为界将关键码划分为：{k0, k1, ..., ks-1}, {ks}, {ks+1, ..., km-1}
* 关键码ks上升一层，并分裂，以所得的两个节点为左右孩子

![dXktVU.png](https://s1.ax1x.com/2020/08/31/dXktVU.png)



插入示例：

1.无需分裂

![dXkjRs.gif](https://s1.ax1x.com/2020/08/31/dXkjRs.gif)

2.分裂一次

![dXkzMq.gif](https://s1.ax1x.com/2020/08/31/dXkzMq.gif)

3.分裂两次

![dXASs0.gif](https://s1.ax1x.com/2020/08/31/dXASs0.gif)

4.分裂到根

![dXAki4.gif](https://s1.ax1x.com/2020/08/31/dXAki4.gif)



## 4. 删除

算法：

```c++
bool BTree<T>::remove( const T & e ) {
    BTNodePosi(T) v = search( e );
    if ( ! v ) return false; //确认e存在
    Rank r = v->key.search(e); //e在v中的秩
    if ( v->child[0] ) { //若v非叶子,则
    BTNodePosi(T) u = v->child[r + 1]; //在右子树中一直向左,即可
    while ( u->child[0] ) u = u->child[0]; //找到e的后继(必属于某叶节点)
    v->key[r] = u->key[0]; v = u; r = 0; //并与之交换位置
    } //至此,v必然位于最底层,且其中第r个关键码就是待删除者
    v->key.remove( r ); v->child.remove( r + 1 ); _size--;
    solveUnderflow( v ); return true; //如有必要,需做旋转或合并
}
```

删除算法首先在B树中查找关键码e，确认e存在。如果e位于非叶节点，则需要将它与它的**后继**交换位置，这样待删除节点必然位于页节点。然后将页节点中的关键码e删除，并删除一个空指针。

由于B树对关键码的最少数目有要求，所以删除可能导致叶节点中的关键码个数发生下溢，此时需要进行旋转或合并操作来进行修复。

**旋转**：

旋转的思想是在发生下溢的节点“左顾右盼”，如果它的左兄弟或者右兄弟中有足够多(至少`ceil(m/2)` )个的关键码，则从它的兄弟中“借出”一个关键码。

![dXkGrV.png](https://s1.ax1x.com/2020/08/31/dXkGrV.png)

算法：

* 若L存在，且至少包括`ceil(m/2)`个关键码
  * 将P中的分界关键码y移入V中（作为最小关键码）
  * 将L中最大的关键码x移入P中（取代原关键码y）
* 若R存在，且至少包含`ceil(m/2)`个关键码
  * 也可旋转，完全对称



**合并**：

![dXk8K0.png](https://s1.ax1x.com/2020/08/31/dXk8K0.png)

算法：

* L和R或不存在，或均不足`ceil(m/2)`个关键码——即便如此，L和仍必有其一，且恰含`ceil(m/2)`-1个关键码
* 从P中抽取介于L和V之间的分界关键码y
  * 通过y做粘接，将L和V合成一个节点
  * 同时合并此前y的孩子引用
* 此处下溢得到修复，但可能导致P下溢（此时继续修复操作）



删除示例：

1.直接删除

![dXACZT.gif](https://s1.ax1x.com/2020/08/31/dXACZT.gif)

2.旋转

![dXAioF.gif](https://s1.ax1x.com/2020/08/31/dXAioF.gif)

3.单次合并

![dXAAJJ.gif](https://s1.ax1x.com/2020/08/31/dXAAJJ.gif)

4.多次合并

![dXAEW9.gif](https://s1.ax1x.com/2020/08/31/dXAEW9.gif)











