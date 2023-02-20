# CentOS使用iso镜像作为私有源

小计：

有时候在我们本地搭建一些Linux上的程序运行环境或者安装一些软件的时候，难免会遇到需要使用yum方式安装一些依赖库，但是苦于没有网，无法下载依赖库软件的情况。又或者是在机房中无法连接外网的情况下需要安装一大堆依赖的基础软件。

1.虚拟机添加CD镜像

2.创建挂载路径 /mut/cdrom

```
[root@localhost /]# mkdir -pv /mut/cdrom
```

3.挂载CD镜像到 /mut/cdrom

```
[root@localhost cdrom]# mount /dev/cdrom /mut/cdrom/
mount: /dev/sr0 写保护，将以只读方式挂载
```

4.查看挂载是否成功 df -h

```
[root@localhost cdrom]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
devtmpfs                 898M     0  898M    0% /dev
tmpfs                    910M     0  910M    0% /dev/shm
tmpfs                    910M  9.6M  901M    2% /run
tmpfs                    910M     0  910M    0% /sys/fs/cgroup
/dev/mapper/centos-root   11G  2.5G  8.6G   22% /
/dev/sda1               1014M  151M  864M   15% /boot
/dev/mapper/centos-home  4.0G   33M  4.0G    1% /home
tmpfs                    182M     0  182M    0% /run/user/0
/dev/sr0                 4.4G  4.4G     0  100% /mut/cdrom
```

5.备份yum源

​           5.1进入yum源目录

```
[root@localhost /]# cd /etc/yum.repos.d/
```

​​            5.2 创建备份文件夹backup

```
[root@localhost yum.repos.d]# mkdir -pv backup
```

​ ​           5.3 备份到backup文件夹

```
[root@localhost yum.repos.d]# mv ./*.repo ./backup
```

6.创建yum更新源local.repo

```
[root@localhost yum.repos.d]# vi local.repo
```

​ ​           6.1编写local.repo yum源

```
[LocalRepo]
name=LocalRepo
baseurl=file:///mut/cdrom/
enabled=1
gpgcheck=0
```

命令含义：

\[LocalRepo] :表示一个yum源配置段的名称，可以自定义。

name:表示该yum源的名称 baseurl:表示yum源的目录，使用file:///表示指向的是本地文件系统上的目录，注意：有三个斜杠。 enabled:表示该yum配置段是否生效，1表示生效，0表示无效 gpgcheck:表示是否对yum源指定的软件包进行安全校验，0表示不校验，本地挂载的镜像可以认为软件就是安全的，不必校验；

7.清除yum源的缓存

```
[root@localhost yum.repos.d]# yum clean all
已加载插件：fastestmirror
正在清理软件源： LocalRepo
Cleaning up list of fastest mirrors
Other repos take up 169 M of disk space (use --verbose for details)

```

8.查看加载的yum源，如果列出很多，说明加载成功

```
[root@localhost yum.repos.d]# yum list all
```

9.卸载挂载目录

```
umount /mut/cdrom/
```















参考链接：

{% embed url="https://www.jb51.net/article/141402.htm" %}
