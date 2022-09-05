# kms命令激活office

使用大客户版的office软件，早期在msdn上下载的带vol标示的，

### 1.进入到自己电脑office的安装目录

一般是`C:\Program Files\Microsoft Office\`这个目录下会有个Office16 或者其他的

office16是office2016，office15就是2013，office14就是2010

里面会有个`OSPP.VBS`的文件，如下图所示:

![image-20220905101443553](https://pic.chjina.com/2022/09/05/OSPP.png)

使用管理员打开CMD命令窗口，

#### 1.1输入你的office的地址，就是你有`OSPP.VBS`的那个目录

```
cd "C:\Program Files (x86)\Microsoft Office\Office16"
```

### 2.设置KMS服务器的地址

```
cscript ospp.vbs /sethst:kms.chjina.com
```

### 3.执行激活命令

```
cscript ospp.vbs /act
```

如有提示以下提示代表激活成功

```
<Product activation successful>
```

```
Installed product key detected - attempting to activate the following product:
SKU ID: b322da9c-a2e2-4058-9e4e-f59a6970bd69
LICENSE NAME: Office 15, OfficeProPlusVL_KMS_Client edition
LICENSE DESCRIPTION: Office 15, VOLUME_KMSCLIENT channel
Last 5 characters of installed product key: GVGXT
<Product activation successful>
---------------------------------------
Installed product key detected - attempting to activate the following product:
SKU ID: d450596f-894d-49e0-966a-fd39ed4c4c64
LICENSE NAME: Office 16, Office16ProPlusVL_KMS_Client edition
LICENSE DESCRIPTION: Office 16, VOLUME_KMSCLIENT channel
Last 5 characters of installed product key: WFG99
<Product activation successful>
---------------------------------------
Installed product key detected - attempting to activate the following product:
SKU ID: e13ac10e-75d0-4aff-a0cd-764982cf541c
LICENSE NAME: Office 15, OfficeVisioProVL_KMS_Client edition
LICENSE DESCRIPTION: Office 15, VOLUME_KMSCLIENT channel
Last 5 characters of installed product key: RM3B3
<Product activation successful>
---------------------------------------
---------------------------------------
---Exiting-----------------------------
```
