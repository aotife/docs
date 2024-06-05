# CentOS网卡配置

### 网卡配置文件

目录地址：`/etc/sysconfig/network-scripts/ifcfg-ens33`

#### 例示：

```

TYPE=Ethernet            //网络类型：Ethernet以太网
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none            //网卡类型 static静态、dhcp动态获取、none不指定
DEFROUTE=yes              //启动默认路由
IPV4_FAILURE_FATAL=no     //不启用IPV4错误检测功能
IPV6INIT=yes              //启用IPV6协议
IPV6_AUTOCONF=yes         //自动配置IPV6地址
IPV6_DEFROUTE=yes         //启用IPV6默认路由
IPV6_FAILURE_FATAL=no     //不启用IPV6错误检测功能
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33                //网卡别名
UUID=c1d713e7-5993-4c4e-8962-cd56d02f5b76       //UUID 可使用uuidgen ens33 自主生成
DEVICE=ens33              //网卡名称
ONBOOT=yes                //开机自动启动网卡
IPADDR=192.168.123.1      //网卡的IP地址
PREFIX=24                 //网卡的IP掩码
GATEWAY=192.168.123.254   //网卡的网关
DNS1=223.5.5.5            //主DNS
DNS2=114.114.114.114      //备用DNS
IPV6_PRIVACY=no
```

### 生成网卡的uuid

命令：`uuidgen ens33`

#### 例示：

```
[root@localhost ~]# uuidgen ens33
4ab75d5e-defa-4935-9c26-039440c26bd3
```

### 查看网卡的uuid

命令：`nmcli con show`

#### 例示：

```
[root@localhost ~]# nmcli con show
NAME   UUID                                  TYPE      DEVICE 
ens33  168fcf94-3f65-49a7-8a80-3e74a622b331  ethernet  ens33
```

### 查看网卡状态

命令：`nmcli device show`

#### 例示：

```
[root@localhost ~]# nmcli device show
GENERAL.DEVICE:                         ens33
GENERAL.TYPE:                           ethernet
GENERAL.HWADDR:                         00:0C:29:96:67:CD
GENERAL.MTU:                            1500
GENERAL.STATE:                          100（已连接）
GENERAL.CONNECTION:                     ens33
GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveConnection/7
WIRED-PROPERTIES.CARRIER:               on 
IP4.ADDRESS[1]:                         192.168.31.190/24
IP4.GATEWAY:                            192.168.31.1
IP4.ROUTE[1]:                           dst = 0.0.0.0/0, nh = 192.168.31.1, mt = 100
IP4.ROUTE[2]:                           dst = 192.168.31.0/24, nh = 0.0.0.0, mt = 100
IP4.DNS[1]:                             192.168.31.1
IP6.ADDRESS[1]:                         fe80::3369:bdb3:a677:821e/64
IP6.GATEWAY:                            --
IP6.ROUTE[1]:                           dst = fe80::/64, nh = ::, mt = 100
IP6.ROUTE[2]:                           dst = ff00::/8, nh = ::, mt = 256, table=255

GENERAL.DEVICE:                         lo
GENERAL.TYPE:                           loopback
GENERAL.HWADDR:                         00:00:00:00:00:00
GENERAL.MTU:                            65536
GENERAL.STATE:                          10（未托管）
GENERAL.CONNECTION:                     --
GENERAL.CON-PATH:                       --
IP4.ADDRESS[1]:                         127.0.0.1/8
IP4.GATEWAY:                            --
IP6.ADDRESS[1]:                         ::1/128
IP6.GATEWAY:                            --
```

### 重启网络

**注：修改完网卡的配置文件(修改IP)必须要重启才生效，**

命令：`service network restart`

#### 例示：

```
[root@localhost ~]# service network restart
Restarting network (via systemctl):                        [  OK  ]
```

### 查看IP地址

命令：`ip add`

#### 例示：

```
[root@localhost ~]# ip add
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:96:67:cd brd ff:ff:ff:ff:ff:ff
    inet 192.168.31.190/24 brd 192.168.31.255 scope global noprefixroute dynamic ens33
       valid_lft 7024sec preferred_lft 7024sec
    inet6 fe80::3369:bdb3:a677:821e/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
