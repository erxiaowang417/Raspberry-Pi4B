## 2.2 docker 
   
   包含两个软件包
   
    dockerd
    luci-app-dockerman
    


#### 1. 根目录扩容

- 调整分区

      # 把软件包列表更新
      opkg update
      # 安装分区软件
      opkg install cfdisk
      # 使用cfdisk进行空间划分
      cfdisk /dev/mmcblk0
      # 格式化分区
      mkfs.ext4 /dev/mmcblk0p? 参考实际分区

      reboot
    
  重启完成后登录路由器web页, 系统 -> 挂载点 -> “生成配置”, 随后在下方"挂载点"区域找到刚才格式化的分区, 点击 “修改”, 选择将它 “作为根文件系统使用”, “启用此挂载点” 勾选, "保存&应用"

- 执行文件转移
    
      # 根目录
      cd /
      mkdir -p /tmp/introot
      mkdir -p /tmp/extroot
      mount --bind / /tmp/introot
      # /dev/mmcblk0p? 同样根据自己实际情况来确定
      mount /dev/mmcblk0p4 /tmp/extroot
      # 本条命令会花费一些时间
      tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
      umount /tmp/introot
      umount /tmp/extroot
      # 上方命令正确执行后, 重启系统
      sync
      reboot
      
  重启完成
#### 2. 安装docker

      dockerd
      luci-app-dockerman
      
#### 3. 配置防火墙
      
      修改允许转发
      iptables -t nat -A POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE
      
#### 4. 安装portainer 镜像
    
    # 拉取Docker 图形化界面 portainer
    docker pull portainer/portainer
    # 创建 portainer 容器
    docker volume create portainer_data
    # 运行
    docker run -d -p 9999:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer

  成功执行后, 便可以使用 路由器IP:9999 访问docker web管理页了
  
