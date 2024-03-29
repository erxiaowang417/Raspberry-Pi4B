# Raspberry-Pi4B
ubuntu server 官方系统 基础配置增加一些功能例ap/docker等

集成功能

- AP
- docker

一、下载官方镜像
======
Ubuntu Server 20.04.4 LTS 64 位
树莓派官方深度定制的硬件驱动与软件程序，官方推荐系统。如果你第一次使用树莓派，请下载这个。

这个版本是 32 位的系统，仅适用于树莓派 3B 3B+ 3A+ 4B Pi 400 CM3 CM3+ CM4 Zero 2 W
    
    地址：https://ubuntu.com/download/raspberry-pi
    （版本自带ssh，区别桌面版无需单独配置）
    
  ![image](https://raw.githubusercontent.com/erxiaowang417/Raspberry-Pi4B/main/ubuntu/png/1.png)
 
    
二、烧录镜像
======

# 1. 烧写tf卡

# 2. 配置ssh

默认启动了SSH支持，无需特意开启。（2016.11起新系统需要通过这个方法开启SSH服务）

三、启动树莓派
======

# 1. 启用root账户

首先我们可以启用root账户来避免每次使用pi账户时都需要添加sudo的麻烦，输入：

    sudo passwd root

出现passwd: password updated successfully 提示成功更新密码，此时，root用户已经启用
进入root账户，输入：

    su root 

# 2. 安装应用配置

    
## 2.1 配置键盘
 
树莓派(raspberry pi)是英国产品，默认键盘布局是英国(GB)，我们用的键盘布局一般是美国(US)的(104键)
    
- 终端输入sudo raspi-config，进入树莓派设置，选择5 localistation Option
- 选择I3 Change Keyboard Layout
- Generic 104-key PC
- Other ->English (US)
- 其余默认
    
## 2.2 配置时间（非必）

- 查看当前系统时间
 
    date -R
- 设置命令

    tzselect
    Asia->china->beijing
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    
## 2.3 配置固定ip

sudo vi /etc/netplan/50-cloud-init.yaml

    network:
       ethernets:
          eth0:
             dhcp4: no
             addresses: [192.168.3.211/24]      #ip
             gateway4: 192.168.3.1
             nameservers:
                addresses: [114.114.114.114, 8.7.7.7]
             optional: true 

生效命令
     
     netplan apply

## 2.4 必要工具

### 先更新apt源（可以换清华大学或者中科大源）
sudo vi /etc/apt/sources.list
#deb [arch=armhf] http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ bullseye main non-free contrib rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ bullseye main non-free contrib rpi
deb [arch=arm64] http://mirrors.tuna.tsinghua.edu.cn/raspbian/multiarch/ bullseye main

apt-key adv --server keyserver.ubuntu.com --recv-keys [key]
gpg --keyserver keyserver.ubuntu.com --recv-keys [key]
gpg --export -armor [key] | sudo apt-key add -

### 编辑 `/etc/apt/sources.list.d/raspi.list` 文件

sudo vi /etc/apt/sources.list.d/raspi.list

deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ bullseye main

注：网址末尾的raspbian重复两次是必须的。因为Raspbian的仓库中除了APT软件源还包含其他代码。APT软件源不在仓库的根目录，而在raspbian/子目录下。

建议更新 Raspbian，包括核心和固件，然后重启系统：

sudo apt update
### 禁用ubuntu自带的dns服务
sudo systemctl stop systemd-resolved.service
sudo systemctl mask systemd-resolved.service
### 删除默认的dns文件
sudo unlink /etc/resolv.conf
sudo nano /etc/resolv.conf
### 打开resolv.conf，添加以下dns
nameserver 223.5.5.5
nameserver 223.6.6.6


四、[创建ap](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/AP/README.MD)
=======
3b/4b 自带wif，使用hostapd配置2.4g 和 5g wifi：
    
###  安装服务
  
    sudo apt install hostapd dnsmasq bridge-utils
### 创建docker bridge（同时也是分享热点bridge）

    //指定一个ip段，ip段不要跟上级路由重合，命名brlan（自定义名字）
    docker network create -d bridge --subnet=192.168.22.1/24 --gateway=192.168.22.1 -o "com.docker.network.bridge.name=brlan" mybr

### 配置文件

    vi /etc/hostapd/hostapd.conf
    
    # 指定网卡
    interface=wlan0
    # 上面创建的bridge
    bridge=brlan
    # "g" simply means 2.4GHz band
    hw_mode=g
    # a是5Ghz 的wifi
    #hw_mode=a
    # wifi频段
    channel=6
    # limit the frequencies used to those allowed in the country
    ieee80211d=1
    # 地区码 这里我填AU
    country_code=AU 
    # 802.11n support
    ieee80211n=1
    # 802.11ac support
    ieee80211ac=1
    # QoS support, also required for full speed on 802.11n/ac/ax
    wmm_enabled=1

    # wifi名称
    ssid=RASPNET_wlan0
    # 1=wpa, 2=wep, 3=both
    auth_algs=1
    # WPA2 only
    wpa=2
    wpa_key_mgmt=WPA-PSK
    rsn_pairwise=CCMP
    #密码
    wpa_passphrase=12345678

    开启wifi
    sudo /usr/sbin/hostapd /etc/hostapd/hostapd.conf  
 
 ### 配置开机启动脚本rc.local
 
 - 编辑 /etc/systemd/system/rc-local.service，没有这个文件就创建一个：

 sudo vi /etc/systemd/system/rc-local.service

    [Unit]
    Description=/etc/rc.local Compatibility
    ConditionPathExists=/etc/rc.local

    [Service]
    Type=forking
    ExecStart=/etc/rc.local start
    TimeoutSec=0
    StandardOutput=tty
    RemainAfterExit=yes
    SysVStartPriority=99

    [Install]
    WantedBy=multi-user.target

- 编辑/etc/rc.local文件：

sudo vi /etc/rc.local

    #!/bin/sh
    #
    # rc.local
    #
    # This script is executed at the end of each multiuser runlevel.
    # Make sure that the script will "exit 0" on success or any other
    # value on error.
    #
    # In order to enable or disable this script just change the execution
    # bits.
    #
    # By default this script does nothing.
    sudo rfkill unblock wlan
    sudo ifconfig wlan0  up

    sudo /usr/sbin/hostapd /etc/hostapd/hostapd.conf  &
    
    exit 0

- 添加权限，使能服务

指令

    sudo chmod +x /etc/rc.local
    sudo systemctl enable rc-local
    sudo systemctl start rc-local.service
    sudo systemctl status rc-local.service
 
 
    
 五、安装docker
 ======= 
 Docker Desktop 可帮助您在 Mac 和 Windows 上轻松构建、共享和运行容器，就像在 Linux 上一样。Docker 处理复杂的设置并允许您专注于编写代码。
 
     curl -fsSL https://get.docker.com -o get-docker.sh
     DRY_RUN=1 sh ./get-docker.sh
    
六、[安装docker](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/Raspbian/docker/README.MD)
=======    
Docker 是一个开源的应用容器引擎，可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口，容器性能开销极低。这对树莓派非常有用，本教程将介绍 Docker 这个工具以及如何在 Raspbian 上安装 Docker。详情目录[详情目录](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/docker) 

关键流程简介：

- [打开网卡混杂模式](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/Raspbian/docker/README.MD)
- [安装docker](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/Raspbian/docker/README.MD)
- [创建网络](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/Raspbian/docker/README.MD)
- docker [基本配置](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/Raspbian/docker/README.MD)

