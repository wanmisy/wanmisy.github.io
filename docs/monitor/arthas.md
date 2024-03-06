## 作用？  
1. 这个类从哪个 jar 包加载的？为什么会报各种类相关的 Exception？  
2. 我改的代码为什么没有执行到？难道是我没 commit？分支搞错了？   
3. 遇到问题无法在线上 debug，难道只能通过加日志再重新发布吗？  
4. 线上遇到某个用户的数据处理有问题，但线上同样无法 debug，线下无法重现！  
5. 是否有一个全局视角来查看系统的运行状况？  
6. 有什么办法可以监控到 JVM 的实时运行状态？  
7. 怎么快速定位应用的热点，生成火焰图？  
8. 怎样直接从 JVM 内查找某个类的实例？  

## 下载
```
curl -O https://arthas.aliyun.com/arthas-boot.jar
java -jar arthas-boot.jar
```

## 使用指南

#### 基础指令
```bash
help 查看命令帮助信息
cls 清空当前屏幕区域
session 查看当前会话的信息
reset 重置增强类，将被 Arthas 增强过的类全部还原，Arthas 服务端关闭时会重置所有增强过的类reset
version 输出当前目标 Java 进程所加载的 Arthas 版本号
history 打印命令历史
quit 退出当前 Arthas 客户端，其他 Arthas 客户端不受影响
stop 和shutdown命令一致stop
shutdown 关闭 Arthas 服务端，所有 Arthas 客户端全部退出
keymap Arthas快捷键列表及自定义快捷键
options 查看或设置Arthas全局开关
```

<div width="100%">
	<img src="images/monitor/arthas.png" alt="双向链表结构" width="500px"/>
</div>

