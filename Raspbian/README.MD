# Raspberry-Pi4B
Raspbian 官方系统 基础配置增加一些功能例ap/docker等

集成功能

- AP
- docker

一、下载官方镜像
======
Raspberry Pi OS 64 位（Raspbian）
树莓派官方深度定制的硬件驱动与软件程序，官方推荐系统。如果你第一次使用树莓派，请下载这个。

这个版本是 32 位的系统，仅适用于树莓派 3B 3B+ 3A+ 4B Pi 400 CM3 CM3+ CM4 Zero 2 W
    
    地址：https://shumeipai.nxez.com/download#os
    
二、烧录镜像
======

# 1. 烧写tf卡

# 2. 配置ssh

最新的Raspbian 2012-10-28已经默认启动了SSH支持，无需特意开启。（2016.11起新系统需要通过这个方法开启SSH服务）
如果因为各种原因，系统没开启SSH服务（从旧系统升级，曾经特意关掉等），可通过sudo raspi-config启用或禁止SSH。

    windows tf卡 根目录 /boot
    
    new-item ssh -type file
   
三、启动树莓派
======

# 1. 启用root账户

首先我们可以启用root账户来避免每次使用pi账户时都需要添加sudo的麻烦，输入：

    sudo passwd root

出现passwd: password updated successfully 提示成功更新密码，此时，root用户已经启用
进入root账户，输入：

    su root 

# 2. 安装应用配置

## 2.1 区域配置

### raspi-config 

    sudo raspi-config 

- 选择 5 Localisation Options ，回车

- 进入配置的二级界面后，选择 L1 Locale ，回车。
- 通过键盘方向键 Locales to be generated: 区域，找到 zh_CN.UTF-8 UTF-8 ， 空格键 进行选中，选中效果是会出现一个 “*” 号。完成选择请回车。
- 选择 en_US.UTF-8
- 键盘的“右箭头”按键，选中 <OK> ，回车，树莓派会回到终端里，界面中会出现几行配置过程信息，直接等待它配置完成即可，然后就会回到一开始的配置界面了。
- 连续按两下“右箭头”按键，选中<Finish> ，回车。
- 如果是第一次配置，应该会让你重启，直接按照提示指引重启就行了。
    
## 2.2 配置键盘
 
树莓派(raspberry pi)是英国产品，默认键盘布局是英国(GB)，我们用的键盘布局一般是美国(US)的(104键)
    
- 终端输入sudo raspi-config，进入树莓派设置，选择5 localistation Option
- 选择I3 Change Keyboard Layout
- Generic 104-key PC
- Other ->English (US)
- 其余默认
    
## 2.3 配置时间（非必）

    sudo dpkg -reconfigure tzdata 
    
    asia(亚洲)->shanghai
    
## 2.4 配置固定ip
两种方案，一 来自于官方发布配置静态ip，二来自于网友分享.由于公司路由器复杂，经常获取到两个ip，故改用第二种

### 2.4.1 方案一
    sudo vi /etc/dhcpcd.conf 
   
interface eth0
static ip_address=10.10.30.221/24   #ip addr
static ip6_address=fd51:42f8:caae:d92e::ff/64
static routers=10.10.30.1           #ip gate
static domain_name_servers=114.114.114.114 8.8.8.8 

eg.

    interface eth0
    static ip_address=10.10.30.221/24   
    static ip6_address=fd51:42f8:caae:d92e::ff/64
    static routers=10.10.30.1           
    static domain_name_servers=114.114.114.114 8.8.8.8 
    
### 2.4.2 方案二(！！！)

192.168.137.8 设置网线ip【不设置会自动分配】 设置网线固定ip方法 在 boot启动盘下 有个cmdline.txt 最前边加一句 ip= xxx.xxx.xxx.xxx    

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

sudo apt-get update
sudo apt-get dist-upgrade
sudo reboot


四、[创建ap](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/AP/README.MD)
=======
3b/4b 自带wif，结合create_ap实现创建2.4g/5g wifi，此软件作者已停止维护，请点击跳转目录[详情目录](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/AP) 
关键流程简介：
    
-  安装 network-manager
-  安装依赖  
-  安装 git    
-  克隆 git 项目
若源无法连接，下载本站[备份文件](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/AP/create_ap.rar)
-  (!!!) 禁用 wpa_supplican 
-  启用热点
-  开机运行 
    
 五、树莓派安装Cockpit Web 控制台  
 =======   
 Cockpit是一个Web控制台，具有易于使用的基于Web的界面，使您可以在服务器上执行管理任务。它也是一个Web控制台，它意味着您也可以通过移动设备访问它。
 
 安装命令
 
    sudo apt install cockpit
 
安装过程将负责设置和启动服务。你可以使用 systemctl 命令来验证状态：

    systemctl status cockpit.socket
    
-----------------------------------------------------------------------------------------
    ● cockpit.socket - Cockpit Web Service Socket
         Loaded: loaded (/lib/systemd/system/cockpit.socket; enabled; vendor preset: enabled)
         Active: active (listening) since Sun 2022-02-27 09:41:57 GMT; 1min 26s ago
       Triggers: ● cockpit.service
           Docs: man:cockpit-ws(8)
         Listen: [::]:9090 (Stream)
          Tasks: 0 (limit: 4165)
            CPU: 28ms
         CGroup: /system.slice/cockpit.socket
-----------------------------------------------------------------------------------------

此时可通过以下地址访问

    ip:9000 访问
    
六、[安装docker](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/docker/README.MD)
=======    
Docker 是一个开源的应用容器引擎，可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口，容器性能开销极低。这对树莓派非常有用，本教程将介绍 Docker 这个工具以及如何在 Raspbian 上安装 Docker。详情目录[详情目录](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/docker) 

关键流程简介：

- [打开网卡混杂模式](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/docker/README.MD)
- [安装docker](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/docker/README.MD)
- [创建网络](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/docker/README.MD)
- docker [基本配置](https://github.com/erxiaowang417/Raspberry-Pi4B/blob/main/docker/README.MD)

