---
title: Ubuntu搭建GIT远程仓库
toc: true
date: 2020-07-13 14:05:19
updated: 2021-07-13 14:05:19
categories:
    - 版本控制
tags:
    - GIT
---

{% raw %}<div class="post-summary">{% endraw %}
在Ubuntu服务器上搭建GIT远程仓库。
{% raw %}</div>{% endraw %}

<!-- more -->
<style type="text/css">
.post-summary { display: none; }
</style>

### 1.创建用户
创建一个单独用户，用来运行GIT服务。
``` bash
sudo adduser YourUserName
```
### 2.导入登陆公钥
将公钥导入到`/home/YourUserName/.ssh/authorized_keys`文件里面。
### 3.初始化Git仓库
``` bash
sudo git init --bare YourRepostoryName.git
```
### 4.更改仓库的OWNER
将新建的仓库owner改为新建的用户git
``` bash
sudo chown -R YourUserName:YourUserName YourRepostoryName.git
```
### 5.禁用git账户的shell登录
编辑`/etc/passwd`文件，把`YourUserName:x:1001:1001:,,,:/home/YourUserName:/bin/bash`改为`YourUserName:x:1001:1001:,,,:/home/YourUserName:/usr/bin/git-shell`。
### 6.Clone远程仓库
``` bash
git clone YourUserName@YourDomain:/home/YourUserName/YourRepostoryName.git
```