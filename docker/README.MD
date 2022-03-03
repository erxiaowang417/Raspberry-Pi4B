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
### 5-1 下载 Docker 图形化界面 portainer
    sudo docker pull portainer/portainer
### 5-2 创建 portainer 容器
    sudo docker volume create portainer_data
### 5-3 运行 portaine

    sudo docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
    
此时可通过以下地址访问

    ip:9000 访问
    
 # 6. Cockpit Web 控制台
 
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
    
 # 7. openwrt




