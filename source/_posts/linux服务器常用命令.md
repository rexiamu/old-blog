---
title: linux 服务器常用命令
date: 2022-09-16 10:23:28
tags:
  - linux
  - mysql
  - nginx
  - tomcat
  - redis
  - java
  - 命令
categories:
  - linux
---

![linux](https://s2.loli.net/2022/09/16/GL3HtisjUQEgDyo.png)

<!-- more -->

## 通用命令

- `netstat -ntlp` 查看端口占用情况
- `netstat -ntulp | grep [port]` 查看指定端口占用情况
- `ps aux | grep [keyword]` 关键字查询相关进程
- `kill -9 [pid]` 通过进程编号杀掉指定进程
- `unzip -o [filename].zip` 解压压缩文件

## mysql

- `mysql -u [uaername] -p` 连接 mysql
- `quit` 退出 mysql
- `mysqldump -u [username] -p [database] > [filename].sql --default-character-set=utf8` 导出数据库
- `source [filename].sql;` 导入 sql
- `create user '[username]'@'[host]' identified by '[passwor]';` 创建 mysql 用户
- `GRANT ALL PRIVILEGES ON [database].[table] TO '[username]'@'[host]' IDENTIFIED BY '[password]' WITH GRANT OPTION;` 授权
- `flush privileges;` 刷新权限

> 注意：【创建 mysql 用户】和【授权】操作后需执行【刷新权限】命令才能生效。

## nginx

- `cd /usr/local/nginx/sbin/` 进入目录
- `./nginx ` 启动
- `./nginx -s stop` 停止
- `./nginx -s quit` 退出
- `./nginx -s reload` 更新配置

## tomcat

- `/usr/local/tomcat/bin` 进入目录
- `nohup ./startup.sh &` 启动 tomcat
- `./shutdown.sh` 关闭 tomcat

## redis

- `/usr/local/redis/bin` 进入目录
- `nohup redis-server &` 启动 redis
- `./redis-cli shutdown` 关闭 redis

## java

- `nohup java -jar [filename].jar &` 启动 jar 包

> 注意：可通过杀掉进程的方式关闭由 jar 包启动的 java 程序
