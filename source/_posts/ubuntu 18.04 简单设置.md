---
title: ubuntu 简单上手
tags :
    - ubuntu
    
date : 2018/12/30

---

## 开机自启动

![](http://hub.syzhong.com:8000/nextcloud/index.php/s/JbTiYyeSdfmfqRm/preview)

<!--more-->
直接搜索 **“启动”**

选择启动应用程序首选项

如果找不到 就去ubuntu的应用商店，搜索启动，挑个顺眼的应用。我好像默认安装了

![](http://hub.syzhong.com:8000/nextcloud/index.php/s/j79AqW75etH5FPn/preview)

安装后打开


![](http://hub.syzhong.com:8000/nextcloud/index.php/s/aGEJF7geo6qSyaz/preview)

然后添加脚本就可以了。

对于18.04而言
最好不要照网上修改rc.loacl 搞不好会崩的，差点重装系统。

## 修改wine应用分辨率

调整“qq轻聊版”字体大小

    WINEPREFIX=~/.deepinwine/Deepin-QQLight deepin-wine winecfg

显示那里改屏幕分辨率就可以

理论上来说 只要是在~/.deepinwine文件夹下的所有win应用都可以这样改大小
把 **Deepin-QQLight**替换成你想改的软件


## 触控板 多点手势

安装Fusuma

    https://github.com/iberianpig/fusuma#installation

安照github上指示一步步做

### 前置
不知道装没装过 ruby 就先装一下

    sudo apt-get install ruby



### 整合步骤

    $ sudo gpasswd -a $USER input

![](http://hub.syzhong.com:8000/nextcloud/index.php/s/cqbGgNS93qHn2HB/preview)

**！！然后reboot或者logout**

这样才能把当前用户使用触控板这个指令生效（我是重启成功的）
    
    $ sudo apt-get install libinput-tools
---------------------
    $ sudo gem install fusuma
------------------
    $ sudo apt-get install xdotool
------------------------
    $ gsettings set org.gnome.desktop.peripherals.touchpad send-events enabled


### 启动

    $ fusuma

### 更新

    $ sudo gem update fusuma

### 个性化配置文件
    
    $ mkdir -p ~/.config/fusuma        # 创建目录 
    
    $ sudo vim ~/.config/fusuma/config.yml # 编辑配置文件
    
    //Tab
**需要重启才能看见效果**


个人配置
``` 
swipe:
  3: 
    left: 
      command: 'xdotool key alt+Tab'
    right: 
      command: 'xdotool key alt+shift+Tab'
    up: 
      command: 'xdotool key ctrl+t'
      threshold: 1.5
    down: 
      command: 'xdotool key ctrl+w'
      threshold: 1.5
  4:
    left:
    #   command: 'xdotool key super+Right'
     command: 'xdotool key super+Left'
    right:
    #   command: 'xdotool key super+Left'
     command: 'xdotool key super+Right'

    up: 
      command: 'xdotool key super+s'
    down: 
      command: 'xdotool key super+d'
pinch:
  2:
    in:
      command: 'xdotool key ctrl+plus'
      threshold: 0.1
    out:
      command: 'xdotool key ctrl+minus'
      threshold: 0.1

threshold:
  swipe: 1
  pinch: 1

interval:
  swipe: 1
  pinch: 1
```























