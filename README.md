# Raspberry-Pi4B
Raspberry Pi4B 官方系统 配置ap/docker等

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
- 首先在这个界面上通过键盘方向键或者鼠标滚动那个 Locales to be generated: 区域，直到找到 zh_CN.UTF-8 UTF-8 ，然后按 空格键 进行选中，选中效果是会出现一个 “*” 号。完成选择请回车。

- 这时会出现下图的“设置系统的默认本地环境”的界面，这里我选择的是 en_US.UTF-8

- 然后在这个界面上，按动键盘的“右箭头”按键，选中 <OK> ，回车，树莓派会回到终端里，界面中会出现几行配置过程信息，不用管，直接等待它配置完成即可，然后就会回到一开始的配置界面了。

- 连续按两下“右箭头”按键，选中<Finish> ，回车。

- 如果是第一次配置，应该会让你重启，直接按照提示指引重启就行了。
等待差不多1分钟吧，再使用ssh方式登录到树莓派上即可。

## 2.2 配置固定ip

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


## 2.3 必要工具

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


四、创建ap
=======
3b/4b 自带wif，结合create_ap实现创建2.4g/5g wifi，此软件作者已停止维护，详情目录[详情目录](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/AP) 

# 1. 安装 network-manager

    sudo apt-get install network-manager
  
# 2. 安装依赖库

    sudo apt-get install util-linux procps hostapd iproute2 iw haveged dnsmasq
    查看是否成功
    systemctl status dnsmasq.service 
  
# 3. 安装 git

    sudo apt-get install git

# 4. 克隆 git 项目

    sudo git clone https://github.com/oblique/create_ap
    cd create_ap
    sudo make install
    
    源无法连接，[下载本站备份文件](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/AP)

# 5. (!!!) 禁用 wpa_supplican 

    sudo vi /etc/dhcpcd.conf

    在文件开头写入 nohook wpa_supplicant，即和 ifconfig wlan0 down 是一样的效果。

# 6. 启用热点
sudo creat_ap wlan0 eth0  [name]  [password]

    eg:  sudo create_ap wlan0 eth0  eda-openwrt  eda1234567
    静默运行
    sudo create_ap wlan0 eth0  eda-openwrt  eda1234567 &

# 7. 开机运行 

    sudo vi /etc/rc.local
    末尾添加
    sudo create_ap wlan0 eth0  eda-openwrt  eda1234567 &
