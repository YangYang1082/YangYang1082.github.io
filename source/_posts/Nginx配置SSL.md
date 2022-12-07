---
title: Nginx配置SSL
toc: true
date: 2020-07-23 14:49:52
updated: 2020-07-23 14:49:52
categories:
  - Web容器
tags:
    - Nginx
    - SSL
    - Let's Encrypt
---

{% raw %}<div class="post-summary">{% endraw %}
在Nginx上配置Let's Encrypt SSL证书。
{% raw %}</div>{% endraw %}

<!-- more -->
<style type="text/css">
.post-summary { display: none; }
</style>

### 1.安装CertBot
``` bash
sudo apt install certbot
```
### 2.安装Nginx插件
``` bash
sudo apt install python3-certbot-nginx
```
### 3.生成证书
``` bash
sudo certbot certonly --nginx
```
### 4.测试自动续期
``` bash
sudo certbot renew --dry-run
```

### 5.安装NGINX

``` bash
sudo apt install nginx
```

### 5.添加配置文件

``` bash
cd /etc/nginx/conf.d
vi 
```