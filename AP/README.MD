创建ap
=======
3b/4b 自带wif，结合create_ap实现创建2.4g/5g wifi，此软件作者已停止维护，详情目录[详情目录](https://github.com/erxiaowang417/ql/tree/main/bilibili) 

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
