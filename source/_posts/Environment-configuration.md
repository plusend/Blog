title: "Environment configuration"
date: 2015-04-19 00:26:19
tags:
- Java
- Git
- configuration
- environment
categories:
- 配置
---
#计算机常用配置
###Java
1. JAVA_HOME

    >C:\Program Files\Java\jdk1.7.0_75
2. CLASSPATH

    >.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
3. path

    >;%JAVA_HOME%\bin;

###Git

1. User name and email

    >git config --global user.name "plusend"

    >git config --global user.email "plusend@163.com"

2. Key

    >ssh-keygen –C "plusend@163.com" –t rsa
