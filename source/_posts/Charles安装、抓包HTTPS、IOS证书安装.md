title: Charles安装、抓包HTTPS、IOS证书安装
toc: true
cover: /gallery/covers/CP77-COVER.jpg
author: Zzzang
tags:
  - https proxy
  - ''
categories:
  - charles
date: 2023-02-06 01:38:00
---
Charles简介
Charles是一个HTTP代理服务器,HTTP监视器,反转代理服务器，当浏览器连接Charles的代理访问互联网时，Charles可以监控浏览器发送和接收的所有数据。它允许一个开发者查看所有连接互联网的HTTP通信，这些包括request, response和HTTP headers （包含cookies与caching信息）。

<!--more-->

一、下载地址：https://www.charlesproxy.com/
1.选择对应的操作系统
2.点击 DOwnload a free trial 进行下载
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68d75cfbab3a4e00156a)

3.安装成功后，打开Charles的主页
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68d35cfbab3a4e001569)

二 Web 抓取HTTPS协议
虽然现在Charles能够直接抓包了，但是https协议的报我们是抓取不了的，需要安装SSL证书才可以
Charles配置操作如下：
2.1，点击顶部菜单栏【Help】–>选择【SSL Proxying】，点击【install Charles Root Certificate 】安装Charles根证书即可；
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68cf5cfbab3a4e001568)
2.2 点击安装证书 -> 本地计算机 -> 将所有证书都放到下列储存 -> 点击下一步完成即可
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68cb5cfbab3a4e001567)
2.3 设置HTTP/HTTPS协议端口
Proxy -> SSL Proxying Settiongs 在弹出选项卡中，勾选【Enable SSL Proxying】点击【add】
添加以下Host和Port

    1. *：*
    2. *：443
    3. *：80
    
     解释：
    	 在Host输入【*】表示接收任何主机
    	 80是http协议的默认端口
    	 443是https协议的默认端口

![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68b15cfbab3a4e001566)
2.4 打开抓取web端设置
Proxy -> Windows Proxy 勾选中即可
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68aa5cfbab3a4e001565)
2.5 访问 www.baidu.com 能够抓取到接口并且接口下有数据表示成功
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68a25cfbab3a4e001564)

    注意：如果接口下抓取是unknown表示失败
    1. 协议未配置
    2. 证书未安装
    3. 防火墙未关闭

三 Charles手机抓包证书安装(IOS)
App抓包
Charles抓包不仅仅可以抓取来在电脑端的HTTP请求，也能够抓取来自App发出的HTTP请求，但是手机抓包需要在电脑端配置下，并且同时需要手机和电脑在同一网络下并且手机VPN也需要关闭

3.1 端口号设置 Proxy -> Proxy Setting 默认端口号 8888
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68895cfbab3a4e001563)
3.2 查看自己IP地址和端口号和下载证书网址
Help -> SSl Proxying -> Install Charles Root Certificate on a mobile Device or Remote Browser
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68745cfbab3a4e001562)
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b686d5cfbab3a4e001561)
3.3手机端设置：
1，打开手机的设置页面；
2，选择【无线局域网】；
3，IOS点击连接的WIFI后面的感叹号，安卓应该是长按连接的WIFI；（注：这里主要以IOS为例）
4，点击【配置代理】–>【手动】；
5，输入本机【IP地址】以及【端口号】，根据自己情况合理配置；
6，点击【存储】
![title](https://www.zzzang.cn/api/file/getImage?fileId=6281f9085cfbab3a4e0015dc)
![title](https://www.zzzang.cn/api/file/getImage?fileId=6281f9315cfbab3a4e0015dd)
![title](https://www.zzzang.cn/api/file/getImage?fileId=6281f95f5cfbab3a4e0015de)
再次说明：服务器IP和端口号需要用到自己的IP和Charles上设置的端口
查看方法：Help -> SSl Proxying -> Install Charles Root Certificate on a mobile Device or Remote Browser（步骤3.2 有截图）
![title](https://www.zzzang.cn/api/file/getImage?fileId=6281f8c25cfbab3a4e0015db)
3.4 检验代理配置是否成功
打开浏览器验证下手机请求，当我们看到Charles里能抓到这个连接，就说明配置没问题，看到unknown，这个不要紧，那是我们没有安装针对手机端的证书，下面继续我们手机端HTTPS证书安装
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b684c5cfbab3a4e00155c)
抓取手机HTTPS协议
通过上面的设置，虽然来自我们手机端的Http协议请求可以抓取到，但是HTTPS协议的包是不能抓取的，需要安装配置证书才可以，现在手机上大多数都已经切到https很少由http协议的了

3.5 Charles配置：
手机打开浏览器输入 chls.pro/ssl,如下提示，点击允许（如果下载失败，请更换浏览器重新在下）
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68455cfbab3a4e00155b)
3.6 进入设置 -> 描述文件与设备管理 -> 点击未安装的证书进行安装
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68265cfbab3a4e001559)
3.7 安装成功后需要在 通用 -> 关于本机 -> 证书信任设置 中勾选Charles证书（针对于IOS手机）
![title](https://www.zzzang.cn/api/file/getImage?fileId=627b68095cfbab3a4e001558)
最后，重新打开charles，手机访问网页/app就可以了

注意事项：
手机和电脑需要在同一网络下
手机VPN需要关闭
不同电脑对应不同证书，所以说你连接其他电脑需要重新下载手机证书
如果抓出来的接口显示Unknown可以把防火墙关闭,再打开charles重新抓取


【转载自CSDN,作者:qq_46022251】https://blog.csdn.net/qq_46022251/article/details/121653287