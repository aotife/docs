# vCenter扩容硬盘

最近给客户巡检设备，发现Vmware虚拟化有报警。

### 问题

提示是Log 硬盘分区告警。如下图

![image-20230821134945757](https://pic.chjina.com/2023/08/21/image-20230821134945757.png)

## 解决办法

### 1.查看硬盘情况

登录vcsa的5480端口查看硬盘情况，

https://192.168.1.1:5480

账号：**root**

密码：**是当时安装vcsa时设置的密码**

如果登录提示密码过期，使用控制台登录vcsa，然后重置密码即可。

![image-20230821141741199](https://pic.chjina.com/2023/08/21/image-20230821141741199.png)

也可以使用ssh 登录系统查看

![image-20230821142121777](https://pic.chjina.com/2023/08/21/image-20230821142121777.png)

### 2.增加硬盘容量

打开vcsa虚拟机，编辑设置，找到硬盘5，编辑硬盘容量，增加到100G（如果无法编辑查看是否有创建的虚拟机快照，有快照删除即可）

![image-20230821142916424](https://pic.chjina.com/2023/08/21/image-20230821142916424.png)

### 3.扩容硬盘容量

ssh登录vcsa，输入一下命令进行扩容硬盘

```
Command> 
Command> shell
Shell access is granted to root
root@192 [ ~ ]# vpxd_servicecfg storage lvm autogrow
VC_CFG_RESULT=0
```

![image-20230821142205826](https://pic.chjina.com/2023/08/21/image-20230821142205826.png)

### 4.查看扩容后的容量

![image-20230821142241060](https://pic.chjina.com/2023/08/21/image-20230821142241060.png)

引用：

https://www.dinghui.org/increasing-the-disk-space-for-the-vcenter-server-appliance.html
