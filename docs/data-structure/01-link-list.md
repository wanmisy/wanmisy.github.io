# 数据结构：链表 Link List  
## 一、数据结构：链表 Link List  
链表是数据元素的线性集合，元素的线性顺序不是由它们在内存中的物理地址给出的。它是由一组节点组成的数据结构，每个元素指向下一个元素，这些节点一起，表示线性序列。

<div align="center">
	<img src="images/data-structure/link-list.png" alt="链表结构" width="500px"/>
</div>

在最简单的链表结构下，每个节点由数据和指针（存放指向下一个节点的指针）两部分组成，这种数据结构允许在迭代时有效地从序列中的任何位置插入或删除元素。
链表的数据结构通过链的连接方式，提供了可以不需要扩容空间就更高效的插入和删除元素的操作，在适合的场景下它是一种非常方便的数据结构。但在一些需要遍历、指定位置操作、或者访问任意元素下，是需要循环遍历的，这将导致时间复杂度的提升。  
优点：和线性表顺序结构相比，链表结构插入，删除操作不需要移动所有节点，不需要初始化容量。  
缺点：搜索时必须遍历节点，含有大量引用，占空间大。  

## 二、链表分类类型  
### 1.单向链表
单链表包含具有数据字段的节点以及指向节点行中的下一个节点的“下一个”字段。可以对单链表执行的操作包括插入、删除和遍历。
<div align="center">
	<img src="images/data-structure/link-list-single.png" alt="单向链表结构" width="500px"/>
</div>

### 2. 双向链表
在“双向链表”中，除了下一个节点链接之外，每个节点还包含指向序列中“前一个”节点的第二个链接字段。这两个链接可以称为'forward（'s'）和'backwards'，或'next'和'prev'（'previous'）。
<div align="center">
	<img src="images/data-structure/link-list-double.png" alt="双向链表结构" width="500px"/>
</div>

### 3. 循环链表
在列表的最后一个节点中，链接字段通常包含一个空引用，一个特殊的值用于指示缺少进一步的节点。一个不太常见的约定是让它指向列表的第一个节点。在这种情况下，列表被称为“循环”或“循环链接”；否则，它被称为“开放”或“线性”。它是一个列表，其中最后一个指针指向第一个节点。
<div align="center">
	<img src="images/data-structure/link-list-circle.png" alt="双向链表结构" width="500px"/>
</div>

## 三、linkList实现

[源码地址](https://gitee.com/vhaha/data-structure.git)  分支: link-list