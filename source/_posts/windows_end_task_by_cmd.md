---
title: Windows下关闭指定端口号的进程
abbrlink: 38420ac9
date: 2020-11-29 14:24:16
tags:
categories:
---

查看指定端口的使用情况
1. 使用命令：

```
netstat -ano | findstr 端口号
```

查看端口时可能会出现以下两种情况，即倒数第二个参数可能是LISTENING，或者TIME_WAIT ， 当参数为 TIME_WAIT时，表示占用此端口的那个进程正在改变状态，稍等一下可能这个进程就结束了。参数为LISTENING 时，就需要手动关闭这个进程了，最后一个参数是这个进程的进程号

2. 运行命令： 执行此命令强制关闭指定进程号的进程

```
taskkill -PID 进程号 -F
```


```
PS F:\01_blog> netstat -ano | findstr 4000
  TCP    0.0.0.0:4000           0.0.0.0:0              LISTENING       16308
  TCP    [::]:4000              [::]:0                 LISTENING       16308
PS F:\01_blog> taskkill -PID 16308 -F
SUCCESS: The process with PID 16308 has been terminated.
PS F:\01_blog>
```

参考：https://blog.csdn.net/eagleuniversityeye/article/details/79985027
