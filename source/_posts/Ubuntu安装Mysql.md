---
title: Ubuntu安装Mysql
toc: true
date: 2020-08-01 15:02:58
updated: 2020-08-01 15:02:58
categories:
  - 数据库
tags:
    - Mysql
---

{% raw %}<div class="post-summary">{% endraw %}
在Ubuntu服务器上安装Mysql数据库，并进行相应配置。
{% raw %}</div>{% endraw %}

<!-- more -->
<style type="text/css">
.post-summary { display: none; }
</style>

### 1.安装Mysql
``` bash
sudo apt install mysql-server
```
### 2.配置Mysql
``` bash
sudo mysql_secure_installation
```
### 3.创建用户
``` bash
create user 'YourUserName' identified by 'YourPassword';
```
### 4.创建数据库
``` bash
create database YourDatabaseName character set utf8mb4 colllate utf8mb4_bin;
```
### 5.授予权限
``` bash
grant all privileges on YourDatabaseName.* to YourUserName;
```