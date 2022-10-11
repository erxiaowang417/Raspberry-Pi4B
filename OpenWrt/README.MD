<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>目录</summary>
  <ol>
    <li>
      <a href="#下载魔改镜像">下载</a>
    </li>
    <li><a href="#烧录镜像">烧录</a></li>
    <li><a href="#基本设置">基本设置</a></li>
    <li>
      <a href="#安装应用配置">安装应用配置</a>
      <ul>
        <li><a href="#kms服务器">KMS服务器</a></li>
      </ul>
      <ul>
        <li><a href="#docker">docker</a></li>
      </ul>
    </li>
  </ol>
</details>

## 下载魔改镜像

OpenWrt系统已经对树莓派4b做了更加完善的兼容，目前我在某代码托管平台上无意发现了一个openwrt的编译项目。

这个版本是 64 位的系统，仅适用于树莓派 3B 3B+ 3A+ 4B Pi 400 CM3 CM3+ CM4 Zero 2 W
    
    地址：https://github.com/SuLingGG/OpenWrt-Rpi/releases/tag/bcm27xx-bcm2711

## 烧录镜像


- 烧写tf卡
将写入固件的tf卡插入树莓派4b中，开机(禁止链接网线)， 会自动出现名为Openwrt的热点：

## 基本设置
======
### 1. 配置网络

#### 1. 接口
![image](https://raw.githubusercontent.com/erxiaowang417/Raspberry-Pi4B/main/OpenWrt/png/1-1.png)

    修改LAN口，取消eth0绑定，取消桥街街口

    新建WAN
        配置静态ip
        取消dchp
        物理设置-接口-eth0
        防火墙 - WAN
#### 2. DHCP/DNS
![image](https://raw.githubusercontent.com/erxiaowang417/Raspberry-Pi4B/main/OpenWrt/png/1-3.png)

    取消唯一授权

#### 3. 防火墙
![image](https://raw.githubusercontent.com/erxiaowang417/Raspberry-Pi4B/main/OpenWrt/png/1-4.png) 

    接受转发

由此，基础配置完成  

## 安装应用配置


### kms服务器
   KMS服务器激活WINDOWS+OFFICE
   安装两个软件包
   
    luci-app-vlmcsd
    vlmcsd
- [配置详情跳转连接](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/OpenWrt/kms/readme.md)

### docker 
 
- 根目录扩容
- 安装docker
- 配置防火墙

        图形化界面 portainer
        青龙
        
- [配置详情跳转连接](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/OpenWrt/docker/readme.md)
    
    

