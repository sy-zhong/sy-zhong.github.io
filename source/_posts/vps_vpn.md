---
title: vps配置-vpn
tags :
    - vps
    - vpn
date :
---
# 选择一个vps
	首先给个忠告，要货比三家，并且不要心急，vps就在那里，晚点买并不会怎么样
------

##一、 vultr
	比较不错的vps，也是我选择的vps，主要靠韬哥推荐（和好看的界面）
	本来只需2.5美元的月租，现在要3.5美元了，2.5美元套餐只提供ipv6，运营商怎么可能会支持。没办法只能选3.5美元的。
	
![](http://hub.syzhong.com:8000/nextcloud/index.php/s/7JFSzQ6JosX67w9/preview)
	
<!--more-->
##二、搬瓦工（BandwagonHost）
	有一个方案性价比比较高，$19.9/year,平均￥12/month,如果单纯只是搭个vpn的话，性价比非常之高。但目前这个套餐售罄了，要买只能等了，等不及就直接vultr$3.5上车吧。
	
------
个人采用的是vultr，教程比较多，可以向韬哥请教，还可以搭个个人网盘+下载器去消耗用不完的流量。

# 正式搭建
## 注测

	这个就不详细说了，简单的。

## 开设服务器
### 1.选择节点
![](http://hub.syzhong.com:8000/nextcloud/index.php/s/rAar2C4z5PQ7GtC/preview)
	
	随便选吧，一般来说选Los  Angeles（洛杉矶），Seattle（西雅图）等美国站点，不过最终还是按实际结果来。
### 2.选择服务器类型
	我选择的是64位centos7，之后的架设都是在此基础上的。

![](http://hub.syzhong.com:8000/nextcloud/index.php/s/j3Jt3SWSSFLfQ7S/preview)

###3.选择套餐
![](http://hub.syzhong.com:8000/nextcloud/index.php/s/ij8ZgfLiPcgBLdR/preview)

	不说了，选 $3.5


### 4.其他操作

	其他现在都不用关心，之后要是搭建其他东西，你就会明白的。
	要填的就最后一行
	
![](http://hub.syzhong.com:8000/nextcloud/index.php/s/rjjdnPJLb72ZNex/preview)
	
	填你的主机名，随便填，就是用于辨识你的vps。
------------

## 登不上
好，现在你刚刚创建了一台vps，但征途还未结束，因为你可能发现你用xshell等远程登录软件登不上，多半ip被封了。
怎么看有没有被封呢？

给两个扫描端口的网站，
[国内端口扫描](http://tool.chinaz.com/port/)
[国外端口扫描](https://www.yougetsignal.com/tools/open-ports/)

先扫国外端口
ip:填你的ip   端口:22（默认就是22）
![](http://hub.syzhong.com:8000/nextcloud/index.php/s/jcT4PNdgqnxS6yC/preview)

	一般都是开的，要是在这步就炸了，就重新删vps再重新开始吧,也可以改防火墙，但有点麻烦，还是删vps吧。

再扫国内端口
![](http://hub.syzhong.com:8000/nextcloud/index.php/s/yDFpSraRB8zwGSZ/preview)

	这里可能22端口会关上，如果关闭了，那就删vps再来吧，不然这个vps就是可用的。
## 登录

如果一切顺利，那么你就拥有了一台可以远程登录的主机。
windows 用xshell等工具登录。
linux 直接用ssh指令登录

	ssh root@yourip


![](http://hub.syzhong.com:8000/nextcloud/index.php/s/Q2wL2JoEFGBgdYe/preview)
	
	密码在server details 里 点进去就能找到

如果觉得用账号密码登录太烦，可以用秘钥登录。
详见 ![]() 

## 搭建vpn（服务端）

这里都使用了一键脚本 感谢大佬  （参考韬哥的博客）

### 安装shadowsocks
	wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
	chmod +x shadowsocks.sh
	./shadowsocks.sh 2>&1 | tee shadowsocks.log

注意在过程中记下**密码**,**加密方式**,**端口**，最终有参数，最好截个图。
	
卸载:
	./shadowsocks.sh uninstall
启动：
	/etc/init.d/shadowsocks start
停止：
	/etc/init.d/shadowsocks stop
重启：
	/etc/init.d/shadowsocks restart
状态：
	/etc/init.d/shadowsocks status

### 锐速加速

一键安装

	wget -N --no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh //安装

>我折腾了好久，锐速的内核，最后莫名其妙就好了，待我好好琢磨琢磨,之后更新


好，目前为止服务端的配置已经结束了

## 使用vpn（客户端）
目前用的系统是ubuntu18.04lts，之前用的win都分享一下吧。

### windows
	
	直接下windows的shadowsocks的客户端，把你的参数都填入。

### ubuntu
配置有点麻烦

#### shadowsocks安装

	apt-get install python-pip
	pip install shadowsocks

#### shadowsocks配置

	也可以装图形界面，但相信我，会很不爽的。
	
	sudo vi /etc/shadowsocks.json
	
然后写入
	
	{
	  "server":"my_server_ip",
	  "local_address": "127.0.0.1",
	  "local_port":1080,
	  "server_port":my_server_port,
	  "password":"my_password",
	  "timeout":300,
	  "method":"aes-256-cfb"
	}
	
启动
	sslocal -c /etc/shadowsocks.json
	
如果需要开机启动，详情。
	添加  sslocal -c /etc/shadowsocks.json

#### 浏览器chrome代理设置
	设置到这里，但还是不能科学上网，你需要用插件代理chrome。
	安装SwitchyOmega插件
	Github 下载 SwitchyOmega：![下载地址](https://github.com/FelisCatus/SwitchyOmega/releases/)  下载crx文件
	chrome 安装 插件，如果不让装，就用开发者模式，打开crx解压后的文件夹。
	下载官方的配置文件：![下载地址](https://github.com/FelisCatus/SwitchyOmega/wiki/GFWList.bak)
	然后导入，如果你的本地地址是1080，没有改动的话，是可以直接用的。
	
	然后直接重启，观察效果。
	
	如果没有成功，就检查以上每个步骤，多重启，多sslocal。


		

# 尾声
	到现在，不出意外就可以科学上网了，但vps的性能还是略有剩余，500G的流量感觉挺少，但正常使用根本用不完，还有20G的ssd，我是搭建了一个私人网盘nextcloud，再搭配上aris2可以简单榨干存储容量和流量，博客之后放出。



------------
end