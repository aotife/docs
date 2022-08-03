# Windows挂载NFS

### 1.Windows 安装NFS客户端

启用或关闭Windows功能

NFS服务打勾

### 2.挂载NFS

打开CMD终端。

查看共享目录 192.168.1.1 为NFS服务器地址

```
showmount -e 192.168.1.1
```

挂载到服务器 `mount \\服务器地址\目录\目录 挂载的盘符`

```
mount  \\192.168.1.1\volume1\home  Z:

提示成功连接到就表示挂载成功了
```

### 3.取消挂载

取消挂载 `umount 挂载`

```
umount Z:
```
