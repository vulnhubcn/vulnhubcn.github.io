---
title: Vulnhub靶机虚拟机无法获得IP
date: 2023-08-08
tags: ["vulnhub"]
---

## 一般情况

将Kali Linux和[Vulnhub](https://www.vulnhub.com/)靶机通过设置`nat网络`连接。

<!--more-->
### 添加新NAT网络

以VirtualBox 为例，先在VirtualBox *全局设定-网络*  点击加号  *添加新NAT网络*

![](https://www.vulnhub.cn/2023/02/02/1.webp)

### 设置NAT网络

将Kali Linux和靶机网络分别设置为`NAT网络`，界面名称选刚才设置的`NatNetwork`

![](https://www.vulnhub.cn/2023/02/02/3.webp)

![](https://www.vulnhub.cn/2023/02/02/2.webp)

### 内网扫描

Kali Linux上执行命令`sudo arp-scan -l`进行C段arp扫描

正常情况下，靶机网卡信息对的上，且获得`nat`网络IP

![](https://www.vulnhub.cn/2023/02/02/4.webp)

## 异常情况

靶机无法获得IP，Kali Linux咋扫也扫不到靶机IP

### 原因分析

多数情况是因为网卡名称对不上导致

### 处理异常

1. 重启系统，鼠标不停点进虚拟机同时不停按下键，停留开机 grub 选择菜单那里，按上键到第一行按`e`。

   ![](https://www.vulnhub.cn/2023/02/02/5.webp)

2. 将光标移动到`linux`开头的那一行，将原来的“`ro`”改为“`rw`”，在后面追加`init=/bin/sh`，（若需更好的调试，可将`quiet`删去），改完后按 Ctrl+X或F10 启动系统。

   ![](https://www.vulnhub.cn/2023/02/02/6.webp)

   改成

   ![](https://www.vulnhub.cn/2023/02/02/7.webp)

3. 成功进入系统。

   看到了熟悉的 # 输入`ip a`

   ![](https://www.vulnhub.cn/2023/02/02/8.webp)

   注意看箭头部分的网卡名称`enp0s17`，记下来

4. 修改配置文件

   - Ubuntu系统新版本

   需修改的文件`/etc/netplan/*.yaml` 

   ![](https://www.vulnhub.cn/2023/02/02/9.webp)

   修改倒数第二行网卡名称为自己的网卡名称

   ![](https://www.vulnhub.cn/2023/02/02/10.webp)

   - Debian系统 和Ubuntu系统旧版本（无`/etc/netplan/*.yaml` ）

   需修改文件`/etc/network/interfaces` ，修改两处网卡名称为自己的网卡名称

   ![](https://www.vulnhub.cn/2023/02/02/11.webp)

5. 重启完成
