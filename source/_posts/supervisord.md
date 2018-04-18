---
title: 解决supervisord进程导致的队列时差问题
tags: 
  - Laravel
  - 任务队列
  - 8小时
  - supervisor
  - created_at
date: 2017-10-31 15:50:47
categories: 
  - Other
---


最近在处理laravel队列的时候突然发现，每次在线上服务器用dispatch添加的队列所生成的数据的时间总是比北京时间少了8小时，最后花费了很多时间才得以解决。以下是我排查的过程：

1. 检查config/app.php的时区配置；
2. 给create方法添加`created_at`和`updated_at`字段；
3. 检查服务器的系统时区以及服务器的mysql时区配置；
4. 检查生成队列的过程中是否存在修改影响时区的代码
5. 排查supervisord守护进程。

经过一系列的排查，以及对比本地与服务器的运行结果，发现不是代码上的问题，也不是服务器上的时区问题，而是之前一个老旧的supervisord进程缓存的配置问题。
通过`ps -ef | grep supervisor`找到关于supervisord的一些进程：

```
root      9616  9559  0 15:02 pts/3    00:00:00 grep --color=auto supervisor
root     22905     1  0 11:13 ?        00:00:03 /usr/bin/python /usr/bin/supervisord -c /etc/supervisord.conf

```
使用命令`kill -s SIGTERM 22905`终止pid为22905的进程，然后输入命令：

```
supervisord -c /etc/supervisord.conf
```

重新将supervisord指定配置文件，更新新的配置到supervisord

```
supervisorctl update
```
重新启动配置中的所有程序

```
supervisorctl reload
```
最后重启所有进程`supervisorctl restart all`即可
