---
title: CentOS7 网络设置
date: 2016-11-05 17:39:12
tags: [服务器开发]
---


# 1、使用虚拟机安装CentOS，并使用桥接方式链接网络

# 2、编辑网络配置
```
    cd /etc/sysconfig/network-scripts/
    vi ifcfg-eno16777736
```
<!--more-->

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=28406526&auto=0&height=66"></iframe>

**更改**
```
    BOOTPROTO=DHCP -> BOOTPROTO=static
    ONBOOT=no -> ONBOOT=yes
```

**增加**

```
    HWADDR=aa:bb:cc:dd:ee;ff //虚拟机的mac地址,在虚拟机的设置-网络中可以查看
    GATEWAY=192.168.0.1 //网关，需要和主机一致
    IPADDR=192.168.0.222 //虚拟机的ip,保持和主机一个网段
    NETMASK=255.255.255.0 //子网掩码
    DNS1=8.8.8.8 //DNS
    DNS2=114.114.114.114 //DNS
```

1、重启网络服务：service network restart
2、试下：ping www.baidu.com

# 3、安装网络工具
```
    yum install net-tools
```

# 4、安装SSH
```
    yum install openssh-server -y
```

# 5、结语
```
   本以为后面会以后端开发继续往前写的，后面发现其实自己工作重心和目前对自己开发有利的方向还是前端，
   恰巧这几天微信小程序可以开发，转而学习微信小程序了。后端是另一个开始，话说我也是Go的追逐者。
```