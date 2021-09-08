## Redis五种数据类型

### <h3 id="redis_data_structure_1">字符串对象(string)</h3>

字符串对象底层数据结构实现为简单动态字符串（SDS）和直接存储，但其编码方式可以是int、raw或者embstr，区别在于内存结构的不同。
* 1. int编码
字符串保存的是整数值，并且这个正式可以用long类型来表示，那么其就会直接保存在redisObject的ptr属性里，并将编码设置为int，如图：<br/>
![image](https://user-images.githubusercontent.com/87458342/132519704-605f9566-20c2-45c4-b5a3-23faad04d111.png)

* 2. raw编码
字符串保存的大于32字节的字符串值，则使用简单动态字符串（SDS）结构，并将编码设置为raw，此时内存结构与SDS结构一致，内存分配次数为两次，创建redisObject对象和sdshdr结构，如图：<br/>
![image](https://user-images.githubusercontent.com/87458342/132519802-72780b33-00a3-440a-a036-169675c1a079.png)

* 3. embstr编码
字符串保存的小于等于32字节的字符串值，使用的也是简单的动态字符串（SDS结构），但是内存结构做了优化，用于保存顿消的字符串；内存分配也只需要一次就可完成，分配一块连续的空间即可，如图：<br/>
![image](https://user-images.githubusercontent.com/87458342/132519975-152ef3c0-f2e8-4bdb-94f0-15d80070b8d1.png)

* 字符串对象总结：
   * 在Redis中，存储long、double类型的浮点数是先转换为字符串再进行存储的。
   * raw与embstr编码效果是相同的，不同在于内存分配与释放，raw两次，embstr一次。
   * embstr内存块连续，能更好的利用缓存在来的优势
   * int编码和embstr编码如果做追加字符串等操作，满足条件下会被转换为raw编码；embstr编码的对象是只读的，一旦修改会先转码到raw。
 
* 应用场景：
   1. 访问量统计：每次访问博客和文章使用 INCR 命令进行递增。
   2. 一般做一些复杂的技术功能的缓存。

### <h3 id="redis_data_structure_2">列表对象(list)</h3>

列表对象的编码可以是ziplist和linkedlist之一。

* 1. ziplist编码
ziplist编码的列表对象底层实现是压缩列表，每个压缩列表节点保存了一个列表元素。<br/>
![image](https://user-images.githubusercontent.com/87458342/132521213-ac18414b-b7d4-4d29-9368-f5b267395a01.png)
 
* 2. linkedlist编码
linkedlist编码底层采用双端链表实现，每个双端链表节点都保存了一个字符串对象，在每个字符串对象内保存了一个列表元素。<br/>
![image](https://user-images.githubusercontent.com/87458342/132521303-9a8b7ea6-2995-4e9e-85ad-d9692e4427fc.png)
 
* 列表对象编码转换：
1. 列表对象使用ziplist编码需要满足两个条件：一是所有字符串长度都小于64字节，二是元素数量小于512，不满足任意一个都会使用linkedlist编码。
2. 两个条件的数字可以在Redis的配置文件中修改，list-max-ziplist-value选项和list-max-ziplist-entries选项。
3. 图中StringObject就是上一节讲到的字符串对象，字符串对象是唯一个在五大对象中作为嵌套对象使用的。
 
* 应用场景：
做简单的消息队列的功能；最新消息排行等功能（比如朋友圈的时间线）。
 
### <h3 id="redis_data_structure_3">哈希对象(hash)</h3>
哈希对象的编码可以是ziplist和hashtable之一。
 
* 1. ziplist编码
ziplist编码的哈希对象底层实现是压缩列表，在ziplist编码的哈希对象中，key-value键值对是以紧密相连的方式放入压缩链表的，先把key放入表尾，再放入value；键值对总是向表尾添加。<br/>
![image](https://user-images.githubusercontent.com/87458342/132521588-ec193f11-3ee4-41b2-b9a4-7f30405d4977.png)

* 2. hashtable编码
hashtable编码的哈希对象底层实现是字典，哈希对象中的每个key-value对都使用一个字典键值对来保存。<br/>
字典键值对即是，字典的键和值都是字符串对象，字典的键保存key-value的key，字典的值保存key-value的value。<br/>
![image](https://user-images.githubusercontent.com/87458342/132521715-b3cc011a-1f38-473b-821c-ee66364d82a5.png)

* 哈希对象编码转换：
哈希对象使用ziplist编码需要满足两个条件：一是所有键值对的键和值的字符串长度都小于64字节；二是键值对数量小于512个；不满足任意一个都使用hashtable编码。<br/>
以上两个条件可以在Reids配置文件中修改hash-max-ziplist-value选项和hash-max-ziplist-entries选项。
 
* 应用场景：
存储、读取、修改对象属性，比如：用户（姓名、性别、爱好），文章（标题、发布时间、作者、内容）
 
### <h3 id="redis_data_structure_4">集合对象(set)</h3>
 
集合对象的编码可以是intset和hashtable之一。
* 1. intset编码
intset编码的集合对象底层实现是整数集合，所有元素都保存在整数集合中。<br/>
![image](https://user-images.githubusercontent.com/87458342/132521935-98713a61-bc4e-47c5-a79d-168aaa2639f4.png)
 
* 2. hashtable编码
hashtable编码的集合对象底层实现是字典，字典的每个键都是一个字符串对象，保存一个集合元素，不同的是字典的值都是NULL；可以参考java中的hashset结构。<br/>
![image](https://user-images.githubusercontent.com/87458342/132522046-75ce3940-7082-4a56-966b-bf223f2b5bf4.png)
 
* 集合对象编码转换：
集合对象使用intset编码需要满足两个条件：一是所有元素都是整数值；二是元素个数小于等于512个；不满足任意一条都将使用hashtable编码。<br/>
以上第二个条件可以在Redis配置文件中修改et-max-intset-entries选项。
 
* 应用场景：
1. 共同好友
2. 利用唯一性，统计访问网站的所有独立ip
 
### <h3 id="redis_data_structure_5">有序集合对象(zset)</h3>
 
有序集合的编码可以是ziplist和skiplist之一。
* 1. ziplist编码 
ziplist编码的有序集合对象底层实现是压缩列表，其结构与哈希对象类似，不同的是两个紧密相连的压缩列表节点，第一个保存元素的成员，第二个保存元素的分值，而且分值小的靠近表头，大的靠近表尾。<br/>
![image](https://user-images.githubusercontent.com/87458342/132522277-19141251-9509-4edc-8027-e2ecdc385b7b.png)

* 2. skiplist编码
skiplist编码的有序集合对象底层实现是跳跃表和字典两种；<br/>
每个跳跃表节点都保存一个集合元素，并按分值从小到大排列；节点的object属性保存了元素的成员，score属性保存分值；<br/>
字典的每个键值对保存一个集合元素，字典的键保存元素的成员，字典的值保存分值。<br/>
![image](https://user-images.githubusercontent.com/87458342/132522391-e134841e-3548-4f54-8c49-bb3bc4d9492b.png)

* 为何skiplist编码要同时使用跳跃表和字典实现？
跳跃表优点是有序，但是查询分值复杂度为O(logn)；字典查询分值复杂度为O(1) ，但是无序，所以结合连个结构的有点进行实现。<br/>
虽然采用两个结构但是集合的元素成员和分值是共享的，两种结构通过指针指向同一地址，不会浪费内存。
 
* 有序集合编码转换：
有序集合对象使用ziplist编码需要满足两个条件：一是所有元素长度小于64字节；二是元素个数小于128个；不满足任意一条件将使用skiplist编码。<br/>
以上两个条件可以在Redis配置文件中修改zset-max-ziplist-entries选项和zset-max-ziplist-value选项。
 
* 应用场景：
排行榜，取TopN操作。带权重的消息队列。
 
### <h3 id="redis_data_structure_6">总结</h3>

在Redis的五大数据对象中，string对象是唯一个可以被其他四种数据对象作为内嵌对象的；<br/>
列表（list）、哈希（hash）、集合（set）、有序集合（zset）底层实现都用到了压缩列表结构，并且使用压缩列表结构的条件都是在元素个数比较少、字节长度较短的情况下；<br/>

四种数据对象使用压缩列表的优点：
* 1. 节约内存，减少内存开销，Redis是内存型数据库，所以一定情况下减少内存开销是非常有必要的。
* 2. 减少内存碎片，压缩列表的内存块是连续的，并分配内存的次数一次即可。
* 3. 压缩列表的新增、删除、查找操作的平均时间复杂度是O(N)，在N再一定的范围内，这个时间几乎是可以忽略的，并且N的上限值是可以配置的。
* 4. 四种数据对象都有两种编码结构，灵活性增加。
