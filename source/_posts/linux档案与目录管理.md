---
title: linux档案与目录管理
date: 2018-12-21 14:27:02
tags: linux
categories: linux
---
## 相对路径与绝对路径  
绝对路径：路径的写法【一定由根目录/写起】  
相对路径：路径的写法【不是由/写起】  
<!-- more -->
## 目录的相关操作  
. 代表当前目录  
.. 代表上一层目录  
- 代表前一个工作目录  
～ 代表当前用户所在家目录  
～account 代表account用户所在家目录  
#### 常见的处理目录指令
cd : 切换目录  
pwd : 显示当前的目录  
mkdir : 创建一个新的目录  
rmdir : 删除一个空的目录
###### pwd 
pwd -P 显示正确的路径，而不会是链接路径
###### mkdir(建立新的目录)    
```mkdir [-mp] 目录名称```   
-m 设定档案的权限    
-p 创建多层目录  
  
###### rmdir
```rm [-p] 目录```  
-p 删除多层目录的空目录

#### 档案与目录的查看：ls
```(linux)
[root@study ~]# ls [-aAdfFhilnrRSt] 文件或文件夹名称..
[root@study ~]# ls [--color={never,auto,always}] 文件或文件夹名称..
[root@study ~]# ls [--full-time] ..
選項與參數：
-a  ：全部的文件，连同隐藏文件( 开头为 . 的文件) 一起列出来常用)
-A  ：全部的文件，连同隐藏文件，但不包括.和..
-d  ：列出目录本身
-f  ：直接列出结果，而不进行排序 (ls 默认用文件名排序！)
-F  ：根据文件或目录，展示附加信息，例如：
      *:代表可执行文件； /:代表目录； =:代表 socket 文件； |:代表 FIFO 文件；
-h  ：将文件容量以易读的方式(例如 GB, KB 等等)列出來；
-i  ：列出 inode 序号；
-l  ：长资料传列出，包含文件的属性和权限；(常用)
-n  ：列出 UID与GID 而非使用者与群組的名称
-r  ：將排序結果反向輸出
-R  ：子目录內容一起列出来
-S  ：以文件容量大小排序，而不是用文件名排序；
-t  ：时间排序
--color=never  ：不显示颜色；
--color=always ：显示顏色
--color=auto   让系統自行根据设定來判斷是否給予顏色
--full-time    ：以完整時間模式 (包含年、月、日、時、分) 輸出
--time={atime,ctime} ：輸出 access 時間或改變權限屬性時間 (ctime) 
                       而非內容變更時間 (modification time)
```

#### 复制、删除与移动cp、rm、mv
#### basename取文件名
#### dirname 取文件路径

#### 档案内容查看
* cat 从第一行开始显示文件内容  
* tac 从最后一行开始显示文件内容  
* nl 显示的时候，输出行号
* more 分页显示  
* less 分页显示，可往前翻页
* head 显示头几行
* tail 显示尾几行
* od 以二进制形式读取文件

#### 搜索whereis、locate、find
```
find [PATH] [option] [action]
```
