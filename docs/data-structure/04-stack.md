# 数据结构：栈 stack

## 一、 堆栈数据结构
堆栈是一种抽象数据类型，用作元素的集合，具有两个主要的操作：  
* PUSH：将元素添加到集合  
* POP：删除最近添加但尚未删除的元素  

<div align="center">
	<img src="images/data-structure/stack-push.png" alt="PUSH" width="500px"/>
</div>

堆栈是一种 LIFO（后进先出）的线性的数据结构，或者更抽象说是一种顺序集合，push 和 pop 操作只发生在结构的一端，称为栈顶。这种结构可以很容易地从堆栈顶部取出一个项目，而要到达堆栈更深处的一个项目可能需要先取出多个其他项目。