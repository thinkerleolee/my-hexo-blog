---
title: WebRTC简介与代码架构
date: 2019-01-27 10:00:34
categories:
- 音视频
tags:
- web RTC
---

## 简介
WebRTC，中文全称网页即时通信（Web Real-Time Communication）的缩写，是一个支持网页浏览器进行实时语音对话或视频对话的API。它于2011年6月1日开源并在Google、Mozilla、Opera支持下被纳入万维网联盟的W3C推荐标准。

WebRTC除了是一套API标准，也是Google的一个对WebRTC标准API的实现（网址：https://webrtc.googlesource.com/src）。

我们主要讨论的是Google的WebRTC的NetWork I/O模块。

## 整体架构

![这里写图片描述](https://img-blog.csdn.net/20180515195830885?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RoaW5rZXJsZW8xOTk3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### WebAPI：提供给Web开发者开发基于Web的类视频聊天应用程序的JavaScript API

#### WebRTC C++ API：一个C++开发的API层，提供给浏览器开发者使用来开发JavaScript API

####Transport/Session：
 Session 组件是基于libjingle （会话协商 + NAT穿透组件库 https://developers.google.com/talk/libjingle/developer_guide）开发

- RTP协议栈 ：（Real Time Protocol）

- P2P（ICE + STUN + TURN）：用来实现点对点传输

- Session Management： 用来建立\管理用户会话，这个层Google并没有在WebRTC中给出实现，而把决策权利给了WebRTC开发者。

## 代码架构
这里主要列出网络I/O相关部分的代码

首先下载 WebRTC native代码
```
git clone https://webrtc.googlesource.com/src
```

**之前的libjingle已经整合到了WebRTC项目中，主要由 rtc_base + pc + p2p 组成**
**注意：P2P的关键实现在客户端，libjingle只是客户端实现，TURN等server还得自己实现。**

整体文件树：

![这里写图片描述](https://img-blog.csdn.net/20180515202651554?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RoaW5rZXJsZW8xOTk3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- **api**：WebRTC C++ API，浏览器开发者调用的API
![这里写图片描述](https://img-blog.csdn.net/20180516095800359?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RoaW5rZXJsZW8xOTk3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- **sdk**： 各个平台的sdk代码（Android && IOS），用于视频采集、渲染等

-  **rtc_base**：一些基础组件的封装代码（SOCKET、线程、事件、buffer、CRC校验等）

- **p2p**：P2P穿透相关，turn/stun等，服务器和客户端
![这里写图片描述](https://img-blog.csdn.net/2018051609585953?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RoaW5rZXJsZW8xOTk3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![](https://img-blog.csdn.net/20180516095947847?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RoaW5rZXJsZW8xOTk3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- **pc**：PeerConnection相关
![这里写图片描述](https://img-blog.csdn.net/20180516100458465?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RoaW5rZXJsZW8xOTk3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- **system_wrappers**：系统调用的封装
![这里写图片描述](https://img-blog.csdn.net/20180516100849253?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RoaW5rZXJsZW8xOTk3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)