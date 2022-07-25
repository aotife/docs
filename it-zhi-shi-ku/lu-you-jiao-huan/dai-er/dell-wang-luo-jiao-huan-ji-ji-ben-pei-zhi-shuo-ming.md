# DELL 网络交换机 基本配置说明

## 配置账号密码

Dell 默认的用户名和密码都是 admin，如果希望修改或添加新的用户

账号:**admin** 密码:**passwor\_123**

```
OS10# configure terminal
OS10(config)# username admin password passwor_123 role sysadmin
OS10(config)# end
OS10# write memory
```

## 设置带外管理

```
OS10#
OS10# configure terminal  
OS10(config)# interface mgmt 1/1/1
OS10(conf-if-ma-1/1/1)# no ip address dhcp
OS10(conf-if-ma-1/1/1)# no ipv6 address autoconfig
OS10(conf-if-ma-1/1/1)# exit
OS10(config)# ip vrf management
OS10(conf-vrf)# interface management
OS10(conf-vrf)# exit
OS10(config)# interface mgmt 1/1/1
OS10(conf-if-ma-1/1/1)# ip address 10.0.0.1/24
OS10(conf-if-ma-1/1/1)# no shutdown
OS10(conf-if-ma-1/1/1)# show configuration
!
interface mgmt1/1/1
no shutdown
no ip address dhcp
ip address 10.0.0.1/24
OS10(conf-if-ma-1/1/1)# exit
OS10#OS10(config)# management route 0.0.0.0/0 172.16.1.254 (带外网络默认路由)
OS10(config)#do write
```

## 交换机接口配置

### 设置Accsee模式

例：创建 VLAN10 并将 e1/1/1 接口以 access 模式划入 vlan10.

```
OS10# configure terminal
OS10(config)# interface vlan 10            //创建vlan10
OS10(conf-if-vl-10)# exit
OS10(config)# interface e1/1/1
OS10(conf-if-eth1/1/1)# switchport mode access     //设置为accsee模式
OS10(conf-if-eth1/1/1)# switchport access vlan 10   // 设置为vlan 10通过
OS10(conf-if-eth1/1/1)# no shutdown
OS10(conf-if-eth1/1/1)# do write
```

### Trunk 模式

例：创建 VLAN10 和 20，并将 e1/1/2 接口配置为 trunk 模式且放行 vlan10 和 20.

```
OS10# configure terminal
OS10(config)# interface vlan 10
OS10(conf-if-vl-10)# exit
OS10(config)# interface vlan 20
OS10(conf-if-vl-20)# exit
OS10(config)# interface e1/1/2
OS10(conf-if-eth1/1/2)# switchport mode trunk         //设置为trunk模式
OS10(conf-if-eth1/1/2)# switchport trunk allowed vlan 10,20  //允许vlan10 20 通过
OS10(conf-if-eth1/1/2)# show configuration
!
interface ethernet1/1/2
no shutdown
switchport mode trunk
switchport access vlan 1 (如果你希望 默认vlan 为 2，此处请 switch access vlan 2 )
switchport trunk allowed vlan 10,20
OS10(conf-if-eth1/1/2)# do write memory
```

### 修改默认 VLAN

OS10 默认 VLAN 是 1 且默认所有 trunk 口都 untagged vlan1，如果需要修改其他 VLAN 为 默认 VLAN

```
OS10# configure terminal
OS10(config)# interface vlan 4093
OS10(conf-if-vl-4093)# exit
OS10(config)# default vlan-id 4093
OS10(config)# do show vlan
Codes: * - Default VLAN, M - Management VLAN, R - Remote Port Mirroring VLANs
Q: A - Access (Untagged), T - Tagged
NUM Status Description Q Ports
1 Inactive
* 4093 Active A Eth1/1/1-1/1/32
OS10(config)#do write
```

## 设置链路聚合

### 静态链路聚合

本例：对 e1/1/49 和 50 接口静态聚合为 PO1，并且配置为 trunk 模式和放行 vlan10,20

```
OS10# configure terminal
OS10(config)# interface port-channel 1              //创建聚合口1
OS10(conf-if-po-1)# switchport mode trunk             //设置为trunk模式
OS10(conf-if-po-1)# switchport trunk allowed vlan 10,20   //放行vlan 10,20
OS10(conf-if-po-1)# no shutdown
OS10(conf-if-po-1)# exit
OS10(config)# interface range e1/1/49-1/1/50           //进入1/1/49-1/1/50接口
OS10(conf-range-eth1/1/49-1/1/50)# channel-group 1      // 加入聚合口1
OS10(conf-range-eth1/1/49-1/1/50)# no shutdown           
OS10(conf-range-eth1/1/49-1/1/50)# end
OS10#write
查看：
OS10# show interface port-channel 1 summary
```

### 动态 LACP 聚合

```
OS10# configure terminal
OS10(config)# interface port-channel 1
OS10(conf-if-po-1)# switchport mode trunk
OS10(conf-if-po-1)# switchport trunk allowed vlan 10,20
OS10(conf-if-po-1)# no shutdown
OS10(conf-if-po-1)# exit
OS10(config)# interface range e1/1/49-1/1/50
OS10(conf-range-eth1/1/49-1/1/50)# channel-group 1 mode active   //设置为lacp模式
OS10(conf-range-eth1/1/49-1/1/50)# no shutdown
OS10(conf-range-eth1/1/49-1/1/50)# end
OOS100#write
查看：
OOS100# show interface port-channel 1 summary
```

## 本地端口镜像

本例中，将 e1/1/1 口的进和出两个方向的流量镜像到 e1/1/2 口进行抓包分析。

```
OS10(config)# interface e1/1/2
OS10(conf-if-eth1/1/2)# no switchport
OS10(conf-if-eth1/1/2)#exit
OS10(config)# monitor session 1             //创建本地镜像组
OS10(conf-mon-local-1)# source interface ethernet 1/1/1 both    //设置源端口（被监控的）
OS10(conf-mon-local-1)# destination interface e1/1/2            //设置目的端口（监控的）
OS10(conf-mon-local-1)# no shut
```

## DHCP Server 配置

本例中，网段为192.168.10.0/24，网关为192.168.10.1，dns为8.8.8.8和9.9.9.9，

```

OS10# configure terminal
OS10(config)# ip dhcp server
OS10(config-dhcp)# pool 192_168_10
OS10(config-dhcp-192_168_10)# lease 0 6 0 （租期为 6 小时）
OS10(config-dhcp-192_168_10)# network 192.168.10.0/24 （此处的/24 前面是没有空格的）
OS10(config-dhcp-192_168_10)# default-router 192.168.10.1 （网关地址）
OS10(config-dhcp-192_168_10)# dns-server 8.8.8.8
OS10(config-dhcp-192_168_10)# dns-server 9.9.9.9
OS10(config-dhcp-192_168_10)# domain-name dhcp.os10.dell （名称，可选）
OS10(config-dhcp-192_168_10)# exit
OS10(config-dhcp)# no disable
OS10(config-dhcp)# do write
查看地址分配表:
OS10#show ip dhcp binding
```

## 一般命令

#### 保存

```
OS10# write //保存交换机配置，请在完成必要配置之后，记得使用此命令保存，否则交换机重启后没保存的配置将丢失
```

#### 修改名称

```
OS10>en
OS10#configure
OS10(config)#hostname ABC //将交换机改名为ABC
OS10(config)#end
OS10#write
```

#### 配置边缘端口

**连接到此交换机的电脑获取DHCP非常慢，或者很慢才能够识别网络，可以考虑配置**

```
OS10#configure
OS10(config)#interface gi1/0/1 //如果对多个端口同时配置，可以使用interface range gi1/0/1-12
OS10(config-if)# spanning-tree portfast
OS10(config-if)#exit
OS10(config)#end
OS10#write
```

#### 开启巨帧流控

**连接ISCSI存储时一般建议配置巨帧和流控**

```
OS10>enable
OS10#configure
OS10(config)#system jumbo mtu 9216 -------------开巨帧
OS10(config)#flowcontrol receive on -------------开流控
OS10(config)#end
OS10#write
```

#### 删除配置（重置交换机）

```
OS10>enable
OS10#write //保存配置
OS10#delete startup-config //删除启动配置文件
Delete startup-config ? (y/n) y //y 确认删除
OS10# reload //重启交换机
Are you sure you want to reload the stack? (y/n) y //y 确认重启
```
