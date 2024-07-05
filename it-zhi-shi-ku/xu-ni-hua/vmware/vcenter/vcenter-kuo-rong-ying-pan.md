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
shell
vpxd_servicecfg storage lvm autogrow
```

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

### 5.重启服务

```
service-control --start --all
```

### 6.重新登录vcsa的5480端口查看状态，

已经扩容成功了，健康状态也显示正常了

### **vCenter Server Appliance 6.7/7.0 的 VMDK的大小/挂载点/目的列表**

| **磁盘** (VMDK) | **默认大小** （微型，具有默认存储大小） | **挂载点**                             | **目的**                                                                        |
| ------------- | ---------------------- | ----------------------------------- | ----------------------------------------------------------------------------- |
| VMDK1         | 12 GB                  | /(10 GB) /boot (132 MB) SWAP (1 GB) | 存储内核映像和引导加载程序配置的目录。                                                           |
| VMDK2         | 1.8 GB                 | /tmp                                | 用于存储 vCenter Server 中的服务生成或使用的临时文件的目录                                         |
| VMDK3         | 25 GB                  | SWAP                                | 系统内存不足时用于交换到磁盘的目录                                                             |
| VMDK4         | 25 GB                  | /storage/core                       | 用于存储 vCenter Server 中 VPXD 进程的核心转储文件的目录                                       |
| VMDK5         | 10 GB                  | /storage/log                        | vCenter Server 和 Platform Services Controller 存储环境的所有日志的目录                    |
| VMDK6         | 10 GB                  | /storage/db                         | VMware Postgres 数据库存储位置                                                       |
| VMDK7         | 5 GB                   | /storage/dblog                      | VMware Postgres 数据库日志记录位置                                                     |
| VMDK8         | 10 GB                  | /storage/seat                       | VMware Postgres 的统计信息、事件、警报和任务 (SEAT) 目录                                      |
| VMDK9         | 1 GB                   | /storage/netdump                    | VMware Netdump Collector 存储库，用于存储 ESXi 转储                                     |
| VMDK10        | 10 GB                  | /storage/autodeploy                 | VMware Auto Deploy 存储库，存储用于 ESXi 主机无状态引导的 thinpackage                         |
| VMDK11        | 10 GB                  | /storage/imagebuilder               | VMware Image Builder 存储库，用于存储 vSphere 映像配置文件、软件库和 VIB 软件包，例如驱动程序 VIB 和更新 VIB。 |
| VMDK12        | 100 GB                 | /storage/updatemgr                  | VMware Update Manager 存储库，用于存储虚拟机和 ESXi 主机的修补程序和更新                            |
| VMDK13        | 50 GB                  | /storage/archive                    | VMware Postgres 数据库的预写日志记录 (WAL) 位置                                           |

引用：

https://www.dinghui.org/increasing-the-disk-space-for-the-vcenter-server-appliance.html
