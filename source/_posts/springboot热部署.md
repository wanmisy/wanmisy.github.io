---
title: springboot热部署
date: 2018-12-25 14:27:44
tags: springboot,spring
categories: springboot
---
开发时，每次修改java代码，项目都需要重启，是不是很麻烦，这里提供一种热部署的方法，使用工具Devtools
```(java)
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-Devtools</artifactId>
	<optional>true</optional>
</dependency>
```

<!-- more -->

```(java)
<build>
	<plugins>
		<plugin>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-maven-plugin</artifactId>
			<configuration>
				<fork>true</fork>
			</configuration>
		</plugin>
	</plugins>
</build>
```

如果是eclipse就已经可以了，如果是idea，还需要以下操作
![](springboot热部署/WX20181225-143302@2x.png)  
commond + shift + A，输入Registry
![](springboot热部署/2C5204C3-8B24-4C69-BED1-9EE9B988CCC4.png)



