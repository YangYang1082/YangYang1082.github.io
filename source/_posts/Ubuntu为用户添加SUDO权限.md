---
title: Ubuntu为用户添加SUDO权限
toc: true
date: 2020-07-20 15:08:48
updated: 2020-07-20 15:08:48
categories:
    - Ubuntu
tags:
    - 权限
---

{% raw %}<div class="post-summary">{% endraw %}
Ubuntu为用户添加SUDO权限
{% raw %}</div>{% endraw %}

<!-- more -->
<style type="text/css">
.post-summary { display: none; }
</style>

### 1.添加写权限
``` bash
chmod u+w /etc/sudoers
```
### 2.修改内容
修改`/etc/sudoers`文件，在`root ALL=(ALL:ALL) ALL`内容下一行添加`YourUserName ALL=(ALL:ALL) ALL`。
### 3.去掉写权限
``` bash
chmod u-w /etc/sudoers
```