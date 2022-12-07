---
title: install_dotnet
date: 2022-12-07 14:17:21
tags:
---

# Install dotnet

``` bash
wget https://packages.microsoft.com/config/ubuntu/22.10/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

sudo apt-get update && \
  sudo apt-get install -y aspnetcore-runtime-7.0
```