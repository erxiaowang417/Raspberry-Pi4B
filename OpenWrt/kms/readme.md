## 2.1 KMS服务器激活WINDOWS+OFFICE
   
   安装两个软件包
   
    luci-app-vlmcsd
    vlmcsd
    
- WINDOWS激活：

#### 1. 打开DOS或powershell，输入slmgr /upk，卸载WINDOWS自带密钥

#### 2. 输入slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX


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
    
#### 3. slmgr /skms 192.168.1.1（路由器IP地址）

#### 4. slmgr /ato

- OFFICE激活：

#### 1.找到你的OFFICE目录

	我的是OFFICE 2016 32位版，目录为：

	C:\Program Files (x86)\Microsoft Office\Office16

	进去这个目录，可以看见有个OSPP.VBS文件

	如果是OFFICE 2016 64位版，目录应为：

	C:\Program Files\Microsoft Office\Office16

#### 2. powershell(管理员)中cd “C:\Program Files (x86)\Microsoft Office\Office16”（双引号中对应你的实际目录）

#### 3. 输入cscript ospp.vbs /sethst:192.168.50.1（你的路由IP）

#### 4. 输入cscript ospp.vbs /act

接下来享受激活的WINDOWS和OFFICE吧（使用系统自身批处理命令激活，因此不可能有后门，不用担心病毒和信息窃取之类的）。如果失败，请检查WINDOWS和OFFICE具体版本信息。
