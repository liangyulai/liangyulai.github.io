---
title: 'Setup Streisand server on Ubuntu 16.04 LTS'
date: 2018-03-13 17:00:00
tags:
  - Streisand
  - Vultr
  - Ubuntu
  - VPS
  - BBR
---

# 申请一个 VPS 主机

> Vultr上有2.5美金/月的主机（目前只在 New Jersey 和 Marmia 有）
> 建议安装 Ubuntu 16.04 LTS

# 升级内核到4.10  （内核在 4.10 后的版本才支持 Google BBR ）

* 检查内核版本
```
  uname -r
  4.4.0-109-generic
```

* 如果内核版本低于 4.10 ，则升级内核版本（比如升级到4.13）
```
  sudo apt-cache search linux-image-4 | grep 4.13
  sudo apt-get install linux-image-4.13.0-36-generic
```
> Install the linux headers when you plan to develop and compile on the machine where you've installed Ubuntu.

```
  sudo reboot
  uname -r
  4.13.0-37-generic
```

# 配置 Google BBR

In order to enable the BBR algorithm, you need to modify the sysctl configuration as follows:

```
  echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf
  echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf
  sudo sysctl -p
```

Now, you can use the following commands to confirm that BBR is enabled:
```
  sudo sysctl net.ipv4.tcp_available_congestion_control
```
The output should resemble:
```
  net.ipv4.tcp_available_congestion_control = bbr cubic reno
```
Next, verify with:
```
  sudo sysctl -n net.ipv4.tcp_congestion_control
```
The output should be:
```
  bbr
```
Finally, check that the kernel module was loaded:
```
  lsmod | grep bbr
```
The output will be similar to:
```
  tcp_bbr                16384  0
```

# 安装 Streisand 

安装步骤可以参考文档 [Streisand(中文版)](https://github.com/StreisandEffect/streisand/blob/master/README-chs.md)
 
* 利用 Python 安装 pip 包管理

```
  sudo apt install python-paramiko python-pip python-pycurl python-dev build-essential
```


* 安装 [Ansible](https://www.ansible.com/) 

```
  sudo pip install ansible markupsafe 
```

  > 如果 pip 版本太低，需要先升级 pip 到 9.0.1 以上，并安装 setuptools

``` 
  sudo pip install --upgrade pip 
  sudo pip install setuptools
```


* 安装 [Streisand](https://github.com/StreisandEffect/streisand)

```
  git clone https://github.com/StreisandEffect/streisand.git && cd streisand
  ./streisand
```

* 以非交互式将 Streisand 部署在正在使用中的服务器上运行:

  >但是需要注意的是这个操作是无法挽回的，它将把你正在使用的 VPS 完全转变为网关后，之前如果你在上面搭建博客或者用于测试某些软件，那完成这个操作后，它们将不复存在。

```
  git clone https://github.com/StreisandEffect/streisand.git && cd streisand
  deploy/streisand-local.sh \
    --site-config global_vars/noninteractive/local-site.yml
```


# 检查开放的端口

```
  sudo ufw status
```

 application | protocol | port  
 ------------|----------|------
L2TP/IPsec | UDP |500
L2TP/IPsec | UDP |1701
L2TP/IPsec | UDP |4500
Nginx | TCP | 443
OpenConnect | TCP | 4443
OpenConnect | UDP | 4443
OpenSSH | TCP | 22
OpenVPN | UDP | 53
OpenVPN | TCP | 53
OpenVPN | TCP | 636
OpenVPN | UDP | 8757
Shadowsocks | TCP | 8530
Shadowsocks | UDP | 8530
Tor | TCP | 8443
Tor | TCP | 9443
WireGuard | UDP | 53
WireGuard | UDP | 51820

