---
title: http-chapter1
date: 2018-02-11 14:27:04
tags:
	- http
categories: "Http"
---

# 1.1 使用HTTP协议访问Web
![image](https://booknotepicture-1256085659.cos.ap-beijing.myqcloud.com/http%E5%9B%BE%E8%A7%A3.png)

Web浏览器从**Web服务器端**获取文件资源`(resource)`，从而显示Web页面。
![image](https://booknotepicture-1256085659.cos.ap-beijing.myqcloud.com/Capture2.JPG)
通过**发送请求**获取服务器资源的Web浏览器等，都可称为**客户端(client)**。

>**一些定义**

```
HTTP（HyperText Transfer Protocol) 超文本传输协议
HTML（HyperText Markup Language) 超文本标记语言
WWW（World Wide Web) 万维网
URL（Uniform Resource Locator) 统一资源定位符
```

# 1.2 TCP/IP协议
>定义


```
TCP/IP 是互联网相关的各类协议族的总称。

HTTP 属于它内部的一个子集。

Protocol:计算机与网络设备相互通信的规则，称为协议。
```

## 1.2.1 TCP/IP的分层管理

TCP/IP 协议族按层次分别分为以下 4层：**应用层**、**传输层**、**网络层**和**数据链路层**。

**应用层**：决定了向用户提供应用服务时通信的活动。`FTP（File
Transfer Protocol，文件传输协议），DNS（Domain Name System，域名系统），HTTP协议`。

**传输层**：提供处于网络连接中的两台计算机之间的数据传输。`TCP（Transmission Control Protocol，传输控制协议）和 UDP（User Data Protocol，用户数据报协议）`。

**网络层**：处理在网络上流动的数据包（网络传输的最小数据单位）。该层规定了通过怎样的路径（所谓的**传输路线**）到达对方计算机，并把数据包传送给对方。

**数据链路层**：处理连接网络的**硬件部分**。包括控制操作系统、硬件的设备驱动、NIC（Network Interface Card，网络适配器，即网卡），及光纤等物理可见部分（还包括连接器等一切传输媒介）。

