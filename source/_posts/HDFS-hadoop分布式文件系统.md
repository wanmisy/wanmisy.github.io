---
title: HDFS(hadoop分布式文件系统)
date: 2019-05-22 13:22:14
categories: 大数据
tags: 大数据,hadoop,HDFS
---

## HDFS(hadoop分布式文件系统)

![5c87a10634075](/HDFS-hadoop分布式文件系统/5c87a10634075.jpg)

#### 一、特点

- 可存储超大文件
- 一次写入，多次读取
- 运行在普通廉价的机器



<!-- more -->

#### 二、不适用场景

- 低延迟
- 大量小文件
- 多用户更新
- 结构化数据
- 数据量不大

#### 三、基本命令

- 打印文件列表（ls） 

  ```vim
  标准写法： hadoop fs -ls hdfs:/  #hdfs: 明确说明是HDFS系统路径 
   
  简写： hadoop fs -ls /   #默认是HDFS系统下的根目录 
   
  打印指定子目录： hadoop fs -ls /package/test/ #HDFS系统下某个目录 
  ```

- 上传文件、目录（put、copyFromLocal） 
  put用法：

  ```
  上传新文件： hdfs fs -put file:/root/test.txt hdfs:/   #上传本地test.txt文件到HDFS根目录，HDFS 根目录须无同名文件，否则“File exists” hdfs fs -put test.txt /test2.txt   #上传并重命名文件。 hdfs fs -put test1.txt test2.txt hdfs:/   #一次上传多个文件到HDFS路径。 
   
  上传文件夹： hdfs fs -put mypkg /newpkg    #上传并重命名了文件夹。 
   
  覆盖上传： hdfs fs -put -f /root/test.txt /   #如果HDFS目录中有同名文件会被覆盖 
  ```

  copyFromLocal用法： 

  ```
  上传文件并重命名： hadoop fs -copyFromLocal file:/test.txt hdfs:/test2.txt 
   
  覆盖上传： hadoop fs -copyFromLocal -f test.txt /test.txt 
  ```

  

- 下载文件、目录（get、copyToLocal） 

  ```
  拷贝文件到本地目录： hadoop fs -get hdfs:/test.txt file:/root/ 
   
  拷贝文件并重命名，可以简写： hadoop fs -get /test.txt /root/test.txt 
  ```

  

- 拷贝文件、目录（cp） 

  ```
  从本地到HDFS，同put hadoop fs -cp file:/test.txt hdfs:/test2.txt  
   
  从HDFS到HDFS hadoop fs -cp hdfs:/test.txt hdfs:/test2.txt  hadoop fs -cp /test.txt /test2.txt 
  ```

  

- 移动文件（mv） 

  ```
  hadoop fs -mv hdfs:/test.txt hdfs:/dir/test.txt  hadoop 
  
  fs -mv /test.txt /dir/test.txt
  ```

  

- 删除文件、目录（rm） 

  ```
  删除指定文件 hadoop fs -rm /a.txt  
   
  删除全部txt文件 hadoop fs -rm /*.txt  
   
  递归删除全部文件和目录 hadoop fs -rm -R /dir/ 
  ```

  

- 读取文件（cat、tail） 

  ```
  hadoop fs -cat /test.txt  #以字节码的形式读取 hadoop fs -tail /test.txt 
  ```

  

- 创建空文件（touchz） 

  ```
  hadoop fs - touchz /newfile.txt 
  ```

  

- 创建文件夹（mkdir） 

  ```
  hadoop fs -mkdir /newdir /newdir2    #可以同时创建多个 hadoop fs -mkdir -p 
  
  /newpkg/newpkg2/newpkg3  #同时创建父级目录 
  ```

  

- 获取逻辑空间文件、目录大小（du） 

  ```
  hadoop fs - du /   #显示HDFS根目录中各文件和文件夹大小 hadoop fs -du -h /   #以最大单位显示HDFS根目录中各文件和文件夹大小 
  hadoop fs -du -s /   #仅显示HDFS根目录大小。即各文件和文件夹大小之和  
  ```

  

