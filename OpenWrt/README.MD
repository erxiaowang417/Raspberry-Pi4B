一、下载魔改镜像
======
OpenWrt系统已经对树莓派4b做了更加完善的兼容，目前我在某代码托管平台上无意发现了一个openwrt的编译项目。

这个版本是 64 位的系统，仅适用于树莓派 3B 3B+ 3A+ 4B Pi 400 CM3 CM3+ CM4 Zero 2 W
    
    地址：https://github.com/SuLingGG/OpenWrt-Rpi/releases/tag/bcm27xx-bcm2711

二、烧录镜像
======

# 1. 烧写tf卡
将写入固件的tf卡插入树莓派4b中，开机(禁止链接网线)， 会自动出现名为Openwrt的热点：

三、基本设置
======
# 1. 配置网络

## 1. 接口
![image](https://raw.githubusercontent.com/erxiaowang417/Raspberry-Pi4B/main/OpenWrt/png/1-1.png)

    修改LAN口，取消eth0绑定，取消桥街街口

    新建WAN
        配置静态ip
        取消dchp
        物理设置-接口-eth0
        防火墙 - WAN
## 2. DHCP/DNS
![image](https://raw.githubusercontent.com/erxiaowang417/Raspberry-Pi4B/main/OpenWrt/png/1-3.png)

    取消唯一授权

## 3. 防火墙
![image](https://raw.githubusercontent.com/erxiaowang417/Raspberry-Pi4B/main/OpenWrt/png/1-4.png) 

    接受转发

由此，基础配置完成  
三、安装应用配置
======

## 2.1 KMS服务器激活WINDOWS+OFFICE
   
   安装两个软件包
   
    luci-app-vlmcsd
    vlmcsd

- WINDOWS激活：

第一步、打开DOS或powershell，输入slmgr /upk，卸载WINDOWS自带密钥

第二步、输入slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX

(在上面key列表选择对应版本的Key，也可以搜索找对应版本key)

(安装对应密钥)

常用Windows VL版KMS激活密钥列表：

    Win10专业版KMS： W269N-WFGWX-YVC9B-4J6C9-T83GX

    Win10企业版KMS： NPPR9-FWDCX-D2C8J-H872K-2YT43

    Win10LTSB版KMS： DCPHK-NFMTC-H88MJ-PFHPY-QJ4BJ

    Win10家庭版KMS： TX9XD-98N7V-6WMQ6-BX7FG-H8Q99

    Win10教育版KMS： NW6C2-QMPVW-D7KKK-3GKT6-VCFB2

    Win7专业版KMS： FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4

    Win7企业版KMS： 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH
    
第三步、slmgr /skms 192.168.1.1（路由器IP地址）

第四步、slmgr /ato

- OFFICE激活：

第一步、找到你的OFFICE目录

我的是OFFICE 2016 32位版，目录为：

C:\Program Files (x86)\Microsoft Office\Office16

进去这个目录，可以看见有个OSPP.VBS文件

如果是OFFICE 2016 64位版，目录应为：

C:\Program Files\Microsoft Office\Office16

第二步、powershell(管理员)中cd “C:\Program Files (x86)\Microsoft Office\Office16”（双引号中对应你的实际目录）

第三步、输入cscript ospp.vbs /sethst:192.168.50.1（你的路由IP）

第四步、输入cscript ospp.vbs /act

接下来享受激活的WINDOWS和OFFICE吧（使用系统自身批处理命令激活，因此不可能有后门，不用担心病毒和信息窃取之类的）。如果失败，请检查WINDOWS和OFFICE具体版本信息。

