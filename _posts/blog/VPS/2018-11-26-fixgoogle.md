---
layout: post
title: 解决VPS谷歌学术 We’re sorry…
categories: VPS
description: VPS使用谷歌学术
keywords: VPS,谷歌学术,shadowsocks
---


### 1 出现原因

&emsp;&emsp;Google把你的IP段封了，所以需要用IPv6来访问谷歌学术。注意首先在创建服务器时候要记得勾选支持ipv6。

### 2 解决方案

（1）首先找到谷歌学术对应的ipv6地址，可以在[这里查看](https://raw.githubusercontent.com/lennylxx/ipv6-hosts/master/hosts)（地址字段下面也有）

（2）在服务器上修改host文件，加入相关配置，首先打开host文件
```
 vi  /etc/hosts
```
（3）添加相关字段(按i进入编辑状态)
```
   2404:6800:4008:c06::be scholar.google.com
   2404:6800:4008:c06::be scholar.google.com.hk
   2404:6800:4008:c06::be scholar.google.com.tw
   2401:3800:4001:10::101f scholar.google.cn #www.google.cn
```
添加完成后结果如下所示：

```
[root@vultr ~]# vi /etc/hosts

127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

127.0.0.1 guest
::1       guest

127.0.0.1 vultr.guest
::1       vultr.guest
2404:6800:4008:c06::be scholar.google.com
2404:6800:4008:c06::be scholar.google.com.hk
2404:6800:4008:c06::be scholar.google.com.tw
2401:3800:4001:10::101f scholar.google.cn #www.google.cn

```
添加完成后，可以输入`:wq!`保存并退出。

（4）重启SS服务
```
    /etc/init.d/shadowsocks restart
```



