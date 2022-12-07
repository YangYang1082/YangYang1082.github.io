---
title: create_linux_user
date: 2022-12-07 10:02:51
tags:
---


# Create user

``` bash
adduser {user_name}
```

# Add ssh key

``` bash
cd /home/{user_name}
mkdir .ssh
cd .ssh
vi authorized_keys
```

# Add sudo

``` bash
vi /etc/sudoers
```

Add ``` {user_name} ALL=(ALL:ALL) ALL ``` after ``` root ALL=(ALL:ALL) ALL ```