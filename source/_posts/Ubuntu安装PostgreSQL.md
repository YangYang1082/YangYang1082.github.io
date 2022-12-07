---
title: Ubuntu安装PostgreSQL
toc: true
date: 2021-04-25 14:55:37
updated: 2021-04-25 14:55:37
categories:
  - 数据库
tags:
    - PostgreSQL
---

{% raw %}<div class="post-summary">{% endraw %}
在Ubuntu服务器上安装PostgreSQL数据库，并进行相应配置。
{% raw %}</div>{% endraw %}

<!-- more -->
<style type="text/css">
.post-summary { display: none; }
</style>

### 1.安装PostgreSQL
``` bash
sudo apt install postgresql
```
### 2.登录PostgreSQL
``` bash
sudo su postgres
psql
```
### 3.创建用户
``` bash
create user YourUserName with password 'YourPassword';
```
### 4.创建数据库
``` bash
create database YourDatabaseName;
```
### 5.授予权限
``` bash
grant all on database YourDatabaseName to YourUserName;
```