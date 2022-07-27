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
 二、Microsoft Office 2019 Vol版Gvlk密钥(KMS激活专用) 

 Office Professional Plus 2019：NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP 

 Office Standard 2019：6NWWJ-YQWMR-QKGCB-6TMB3-9D9HK 

 Project Professional 2019：B4NPR-3FKK7-T2MBV-FRQ4W-PKD2B 

 Project Standard 2019：C4F7P-NCP8C-6CQPT-MQHV9-JXD2M 

 Visio Professional 2019：9BGNQ-K37YR-RQHF2-38RQ3-7VCBB 

 Visio Standard 2019：7TQNQ-K3YQQ-3PFH7-CCPPM-X4VQ2 

 Access 2019：9N9PT-27V4Y-VJ2PD-YXFMF-YTFQT 

 Excel 2019：TMJWT-YYNMB-3BKTF-644 FC-RVXBD 

 Outlook 2019：7HD7K-N4PVK-BHBCQ-YWQRW-XW4VK 

 PowerPoint 2019：RRNCX-C64HY-W2MM7-MCH9G-TJHMQ 

 Publisher 2019：G2KWX-3NW6P-PY93R-JXK2T-C9Y9V 

 Skype for Business 2019：NCJ33-JHBBY-HTK98-MYCV8-HMKHJ 

 Word 2019：PBX3G-NWMT6-Q7XBW-PYJGG-WXD33 
