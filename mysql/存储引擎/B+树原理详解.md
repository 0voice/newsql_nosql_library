# B+树原理详解

B+树是B树的一种变体，有着比B树更高的查询性能。一个m阶B树具有如下特征：

1. 根节点至少有两个节点；
2. 每个中间节点都包含k-1个元素和k个孩子，其中m/2<=k<=m；
3. 每一个叶子节点都包含k-1个元素，其中m/2<=k<=m；
4. 所有的叶子节点都位于同一层；
5. 每个节点中的元素从小到大排列，节点当中k-1个元素正好是k个孩子包含的元素的值域划分。

B+树和B树有一些共同特征，但是B+树也具备一些新的特征：

1. 有k个子树的中间节点包含有k个元素（B树中是k-1个元素），每个元素不保存数据，只用来索引，所有数据都保存在叶子节点；
2. 所有的叶子节点中包含了全部元素的信息，及指向含这些元素记录的指针，且叶子节点本身依关键字的大小自小而大顺序链接；
3. 所有的中间节点元素都同时存在于子节点，在子节点元素中是最大（或最小）元素。

我们具体来看一下B+树结构：
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615448-5b0f7e45-d9fb-4ae6-985c-748777b8ad36.png)
<br/>
B+树结构
  
</div>

我们可以直观地看出节点之间含有重复元素，叶子节点还用指针连在了一起，每个父节点中的元素都出现在了子节点中，是子节点中的最大（或最小）元素。
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615491-14816caf-add1-4f5d-bda5-fc9986a04ec8.png)
<br/>
B+树中的元素
  
</div>

如上图，根节点中元素8是子节点2，5，8的最大元素，也是叶子节点6，8的最大元素，根节点元素15是子节点11，15的最大元素，也是叶子节点13，15的最大元素。需要注意的是，根节点的最大元素（此处是15）等同于整个B+树的最大元素。无论插入或删除多少元素，始终要保持最大元素在根节点当中。至于叶子节点，由于父节点的元素都出现在了子节点，所以叶子节点包含了全部元素信息。并且每个叶子节点都带有指向下一个节点的指针，形成了一个有序链表。
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615535-c52d50a2-e1eb-4df5-9aa0-319628289c76.png)
<br/>
叶子节点形成了有序链表
</div>

B+树还有一个至关重要的特点，那就是”卫星数据“的位置，所谓”卫星数据“，指的是索引元素所指向的数据记录（比如数据库中的某一行），在B树中，无论中间节点还是叶子节点都带有卫星数据。
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615566-fab3ddf2-4683-4d2a-ba46-0314b3a9dc9f.png)
B树中的卫星数据
</div>
而在B+树中，只有叶子节点带有卫星数据，其余中间节点仅仅是索引，没有任何数据关联。
 
 <div align="center">
![image](https://user-images.githubusercontent.com/87458342/132615590-30a28c45-4062-423a-9ff2-dac20b03fb63.png)
<br/>
  B+树的卫星数据
</div>

补充一点：在数据库的聚集索引中，叶子节点直接包含卫星数据，在非聚集索引中，叶子节点带有指向卫星数据的指针。
B+树被设计如此，优势主要体现在查询性能上，下面分别通过单元素查询和范围查询举例分析。
单元素查询的时候，B+树会自顶向下逐层查找节点，最终找到匹配的叶子节点，比如我们查找元素3：

 <div align="center">
   
![image](https://user-images.githubusercontent.com/87458342/132615635-ca5e34fe-caf8-41a2-ab7b-db7e2fb2e474.png)
<br/>
第一次磁盘IO
</div>

<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615647-698c024d-c326-49b3-8aea-a2cdf5c5369a.png)
<br/>
第二次磁盘IO
</div>
 <div align="center">
   
![image](https://user-images.githubusercontent.com/87458342/132615658-8af81b90-057c-462a-927a-cf39fba6a522.png)
<br/>
第三次磁盘IO
</div>
  
查询过程看上去跟B树差不多，但还是有两点不同的，首先，B+树中间节点没有卫星数据，只存索引数据，所以同样大小的磁盘页可以容纳更多的节点元素，这就意味着，数据量相同的情况下B+树比B树更加的”矮胖“，相应会减小IO次数。其次，B+树的查询必须最终查找到叶子节点，而B树只要找到匹配元素即可，无论匹配元素处于中间节点还是叶子节点。
因此，B树的查找性能并不稳定，最好的情况是只查根节点即可，最坏的情况是要查到叶子节点，而B+树每一次查找都是稳定的。
下面我们来看看范围查询，先看看B树如何做范围查询的呢？B树只能依靠中序遍历，以查询3到11范围的元素为例：

 <div align="center">
   
![image](https://user-images.githubusercontent.com/87458342/132615743-2963a821-8c43-4f2f-b1a7-7c236b1779e7.png)
<br/>
自顶向下查找到下限3
</div>
 <div align="center">
   
![image](https://user-images.githubusercontent.com/87458342/132615771-61910f3d-ee21-4f19-939a-df55b1d2c356.png)
<br/>
中序遍历到元素6
</div>
 <div align="center">
   
![image](https://user-images.githubusercontent.com/87458342/132615787-f0eb92b7-c3ed-4e25-afee-3d1a49f5b2e3.png)
<br/>
中序遍历到元素8
</div>
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615803-a245f92a-12f7-47bc-89c7-3c758bd9589f.png)
<br/>
中序遍历到元素9
</div>
 <div align="center">
   
![image](https://user-images.githubusercontent.com/87458342/132615828-ec9e2c7b-4904-4c0b-bcc0-000a6b815451.png)
<br/>
中序遍历到元素11，遍历结束

</div>
由此看来，B树的范围查询确实有点繁琐，反观B+树的范围查询则简单的多，只需在链表上做遍历即可：
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615862-999244a7-f428-43cc-bb1e-ac65ef2092be.png)
<br/>
自顶向下，查找到范围下限3

</div>
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615877-3e9fbaed-0362-4d48-9a98-63c62969d332.png)
<br/>
通过链表指针，遍历到元素6，8

</div>
<div align="center">
  
![image](https://user-images.githubusercontent.com/87458342/132615890-a3b3d072-cad7-4359-af5c-d77d2a3dca73.png)
<br/>
通过链表指针，遍历到元素9，11，遍历结束

</div>

如此看来B+树的链表遍历要比B树的中序遍历简单很多的。
综合起来，B+树比B树的优势有三个：
1. 单一节点存储更多的元素，使得查询的IO次数减少；
2. 所有查询都要查找到叶子节点，查询性能稳定；
3. 所有叶子节点形成有序链表，便于范围查询。
