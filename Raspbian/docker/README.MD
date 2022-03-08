docker 安装
=======
Docker 是一个开源的应用容器引擎，可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口，容器性能开销极低。这对树莓派非常有用，本教程将介绍 Docker 这个工具以及如何在 Raspbian 上安装 Docker。详情目录[详情目录](https://github.com/erxiaowang417/Raspberry-Pi4B/tree/main/docker) 

# 1.  打开网卡混杂模式

    sudo ip link set eth0 promisc on 
  
# 2. 安装docker

选择脚本安装
 
    sudo curl -sSL https://get.docker.com | sh
  
# 3. 创建网络

    sudo docker network create -d macvlan \
        --subnet=192.168.1.0/24  \                          #路由地址
        --gateway=192.168.1.1 \                             #路由网关
        -o parent=eth0 \
        -o macvlan_mode=bridge \
        my_eda
 
 docker 网络操作
 
    docker network ls                                       #查看命令      
    docker network  connect  [net] [container]              #连接
    eg. docker network  connect my_eda  openwrt

    docker network  disconnect  [net] [container]           #断开连接
    docker network inspect [net]                            #查看详情 
    eg. docker network inspect  my_eda    

# 4. docker 基本配置

### 4-1 创建docker用户组

    sudo groupadd docker
   
### 4-2 应用用户加入docker用户组

 
    sudo usermod -aG docker ${USER}
    

### 4-3 重启docker服务

    sudo systemctl restart docker

# 5. Docker 图形化界面
### 5-1 拉取 Docker 图形化界面 portainer
    sudo docker pull portainer/portainer
### 5-2 创建 portainer 容器
    sudo docker volume create portainer_data
### 5-3 运行 portaine

    sudo docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
    
此时可通过以下地址访问

    ip:9000 访问
    
    
 # 6. openwrt

### 6-1 拉取 openwrt 镜像

    sudo docker pull sulinggg/openwrt:rpi4 
### 6-2 创建/启动容器

    sudo docker run --restart always --name openwrt  -d --network my_eda --privileged sulinggg/openwrt:rpi4  /sbin/init
    
### 6-2 进入容器并修改相关参数

    sudo docker exec -it openwrt bash               #进入openwrt 容器的SSH

    vi  /etc/config/network                         #修改openwrt网络设置 

----------------------------------------
#以下无需更改

    config interface 'loopback'
            option ifname 'lo'
            option proto 'static'
            option ipaddr '127.0.0.1'
            option netmask '255.0.0.0'
#以下无需更改

    config globals 'globals'
#根据需求更改

    config interface 'lan'
            option type 'bridge'
            option ifname 'eth0'        
            option proto 'static'
            option netmask '255.255.255.0'
            option ipaddr '10.10.30.212'                #这里 不与eth0相同
            option gateway '10.10.30.211'                 #这里 gate
            option dns '10.10.30.211'                     #这里 
#以下无需更改

    config interface 'vpn0'
            option ifname 'tun0'
            option proto 'none'
----------------------------------------

### 6-3 重启网络并检查（docker openwrt 内）
docker openwrt 内

    /etc/init.d/network restart              #重启openwrt的网络
    ip addr                                 #检查网络设置
    exit                                    #退出docker里的openwrt的ssh
    
 ### 6-4 解决树莓派网卡重启后的混杂丢失问题(end)
  
  添加一下信息
  
    sudo vi /etc/dhcpcd.conf
  
------------------------------------------------------------------------

    //#Example static IP configuration:
    interface eth0
    static ip_address=192.168.3.189/24
    #static ip6_address=fd51:42f8:caae:d92e::ff/64
    static routers=192.168.3.1
    static domain_name_servers=192.168.3.1
    #最后添加
    auto macvlan
    iface macvlan inet static
     address 10.10.30.212 
     netmask 255.255.255.0
     gateway 10.10.30.211                    #路由器 IP
     dns-nameservers 10.10.30.211            #路由器 IP
     pre-up ip link add macvlan link eth0 type macvlan mode bridge
     post-down ip link del macvlan link eth0 type macvlan mode bridge
 
------------------------------------------------------------------------
