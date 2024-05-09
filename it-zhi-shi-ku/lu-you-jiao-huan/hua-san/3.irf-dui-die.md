---
description: 华三irf堆叠配置指导
---

# 3 IRF堆叠

## 1.1 IRF简介

IRF（Intelligent Resilient Framework，智能弹性架构）是H3C自主研发的软件虚拟化技术。它的核心思想是将多台设备连接在一起，进行必要的配置后，虚拟化成一台设备。使用这种虚拟化技术可以集合多台设备的硬件资源和软件处理能力，实现多台设备的协同工作、统一管理和不间断维护。

为了便于描述，这个“虚拟设备”也称为IRF。所以，本文中的IRF有两层意思，一个是指IRF技术，一个是指IRF设备。

### 1.1.1 IRF的优点

IRF主要具有以下优点：

· 简化管理。IRF形成之后，用户通过任意成员设备的任意端口都可以登录IRF系统，对IRF内所有成员设备进行统一管理。

· 1:N备份。IRF由多台成员设备组成，其中，主设备负责IRF的运行、管理和维护，从设备在作为备份的同时也可以处理业务。一旦主设备故障，系统会迅速自动选举新的主设备，以保证业务不中断，从而实现了设备的1:N备份。

· 跨成员设备的链路聚合。IRF和上、下层设备之间的物理链路支持聚合功能，并且不同成员设备上的物理链路可以聚合成一个逻辑链路，多条物理链路之间可以互为备份也可以进行负载分担，当某个成员设备离开IRF，其它成员设备上的链路仍能收发报文，从而提高了聚合链路的可靠性。

· 强大的网络扩展能力。通过增加成员设备，可以轻松自如的扩展IRF的端口数、带宽。因为各成员设备都有CPU，能够独立处理协议报文、进行报文转发，所以IRF还能轻松自如的扩展处理能力。

### 1.1.2 IRF的应用

如[图1-1](https://www.h3c.com/cn/d\_202106/1413399\_30005\_0.htm#\_Ref412812873)所示，主设备和从设备组成IRF，对上、下层设备来说，它们就是一台设备——IRF。

图1-1 IRF组网应用示意图

![img](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220519150322.png)

## 2 IRF典型配置举例（BFD MAD检测方式）

### 1. 组网需求

由于网络规模迅速扩大，当前中心交换机（Device A）转发能力已经不能满足需求，现需要在保护现有投资的基础上将网络转发能力提高三倍，并要求网络易管理、易维护。

### 2. 组网图

图1-15 IRF典型配置组网图（BFD MAD检测方式）

![img](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220519151529.png)

### 3. 配置思路

· Device A处于局域网的汇聚层，为了将汇聚层的转发能力提高三倍，需要另外增加三台设备Device B、Device C和Device D。

· 鉴于IRF技术具有管理简便、网络扩展能力强、可靠性高等优点，所以本例使用IRF技术构建网络汇聚层（即在四台设备上配置IRF功能），每台成员设备与上层设备Device E之间均有一条上行链路连接。

· 为了防止IRF链路故障导致IRF分裂，网络中存在两个配置冲突的IRF，需要启用MAD检测功能。本例中我们采用BFD MAD检测方式来监测IRF的状态，并使用成员设备与上层设备间的专用链路传递BFD MAD报文。

### 4. 配置步骤

注：一个设备有两个IRF端口，1和2。相邻的两个设备的端口不要相同，否则不能形成IRF。

如：1号设备的irf-port 1/1物理端口只能对应2号设备的 irf-port 2/2物理端口

![img](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220519151647.png)

#### (1) 配置Device A

\# 根据[图1-15](https://cdn.jsdelivr.net/gh/xyzz-by/imge@main/2021/20220427203415.png)选定IRF物理端口并关闭这些端口。

```
<Sysname> system-view
[Sysname] interface range ten-gigabitethernet 1/0/49 to ten-gigabitethernet 1/0/52
[Sysname-if-range] shutdown
[Sysname-if-range] quit
```

\# 配置IRF端口1/1，并将它与物理端口Ten-GigabitEthernet1/0/49和Ten-GigabitEthernet1/0/50绑定。

```
[Sysname] irf-port 1/1
[Sysname-irf-port1/1] port group interface ten-gigabitethernet 1/0/49
[Sysname-irf-port1/1] port group interface ten-gigabitethernet 1/0/50
[Sysname-irf-port1/1] quit
```

\# 配置IRF端口1/2，并将它与物理端口Ten-GigabitEthernet1/0/51和Ten-GigabitEthernet1/0/52绑定。

```
[Sysname] irf-port 1/2
[Sysname-irf-port1/2] port group interface ten-gigabitethernet 1/0/51
[Sysname-irf-port1/2] port group interface ten-gigabitethernet 1/0/52
[Sysname-irf-port1/2] quit
```

\# 开启Ten-GigabitEthernet1/0/49～Ten-GigabitEthernet1/0/52端口，并保存配置。

```
[Sysname] interface range ten-gigabitethernet 1/0/49 to ten-gigabitethernet 1/0/52
[Sysname-if-range] undo shutdown
[Sysname-if-range] quit
[Sysname] save
```

\# 激活IRF端口下的配置。

```
[Sysname] irf-port-configuration active
```

#### (2) 配置Device B

\# 将Device B的成员编号配置为2，并重启设备使新编号生效。

```
<Sysname> system-view
[Sysname] irf member 1 renumber 2
Renumbering the member ID may result in configuration change or loss. Continue? [Y/N]:y
[Sysname] quit
<Sysname> reboot
```

\# 根据[图1-15](https://cdn.jsdelivr.net/gh/xyzz-by/imge@main/2021/20220427203415.png)选定IRF物理端口并进行物理连线。

\# 重新登录到设备，关闭选定的所有IRF物理端口。

```
<Sysname> system-view
[Sysname] interface range ten-gigabitethernet 2/0/49 to ten-gigabitethernet 2/0/52
[Sysname-if-range] shutdown
[Sysname-if-range] quit
```

\# 配置IRF端口2/1，并将它与物理端口Ten-GigabitEthernet2/0/49和Ten-GigabitEthernet2/0/50绑定。

```
[Sysname] irf-port 2/1
[Sysname-irf-port2/1] port group interface ten-gigabitethernet 2/0/49
[Sysname-irf-port2/1] port group interface ten-gigabitethernet 2/0/50
[Sysname-irf-port2/1] quit
```

\# 配置IRF端口2/2，并将它与物理端口Ten-GigabitEthernet2/0/51和Ten-GigabitEthernet2/0/52绑定。

```
[Sysname] irf-port 2/2
[Sysname-irf-port2/2] port group interface ten-gigabitethernet 2/0/51
[Sysname-irf-port2/2] port group interface ten-gigabitethernet 2/0/52
```

\# 开启Ten-GigabitEthernet2/0/49～Ten-GigabitEthernet2/0/52端口，并保存配置。

```
[Sysname] interface range ten-gigabitethernet 2/0/49 to ten-gigabitethernet 2/0/52
[Sysname-if-range] undo shutdown
[Sysname-if-range] quit
[Sysname] save
```

\# 激活IRF端口下的配置。

```
[Sysname] irf-port-configuration active
```

(3) Device A和Device B间将会进行主设备竞选，竞选失败的一方将重启，重启完成后，IRF形成。

#### (4) 配置Device C

\# 将Device C的成员编号配置为3，并重启设备使新编号生效。

```
<Sysname> system-view
[Sysname] irf member 1 renumber 3
Renumbering the member ID may result in configuration change or loss. Continue? [Y/N]:y
[Sysname] quit
<Sysname> reboot
```

\# 根据[图1-15](https://cdn.jsdelivr.net/gh/xyzz-by/imge@main/2021/20220427203415.png)选定IRF物理端口并进行物理连线。

\# 重新登录到设备，关闭选定的所有IRF物理端口。

```
<Sysname> system-view
[Sysname] interface range ten-gigabitethernet 3/0/49 to ten-gigabitethernet 3/0/52
[Sysname-if-range] shutdown
[Sysname-if-range] quit
```

\# 配置IRF端口3/1，并将它与物理端口Ten-GigabitEthernet3/0/49和Ten-GigabitEthernet3/0/50绑定。

```
[Sysname] irf-port 3/1
[Sysname-irf-port3/1] port group interface ten-gigabitethernet 3/0/49
[Sysname-irf-port3/1] port group interface ten-gigabitethernet 3/0/50
[Sysname-irf-port3/1] quit
```

\# 配置IRF端口3/2，并将它与物理端口Ten-GigabitEthernet3/0/51和Ten-GigabitEthernet3/0/52绑定。

```
[Sysname] irf-port 3/2
[Sysname-irf-port3/2] port group interface ten-gigabitethernet 3/0/51
[Sysname-irf-port3/2] port group interface ten-gigabitethernet 3/0/52
[Sysname-irf-port3/2] quit
```

\# 开启Ten-GigabitEthernet3/0/49～Ten-GigabitEthernet3/0/52端口，并保存配置。

```
[Sysname] interface range ten-gigabitethernet 3/0/49 to ten-gigabitethernet 3/0/52
[Sysname-if-range] undo shutdown
[Sysname-if-range] quit
[Sysname] save
```

\# 激活IRF端口下的配置。

```
[Sysname] irf-port-configuration active
```

(5) Device C将自动重启，加入Device A和Device B已经形成的IRF。

#### (6) 配置Device D

\# 将Device D的成员编号配置为4，并重启设备使新编号生效。

```
<Sysname> system-view
[Sysname] irf member 1 renumber 4
Renumbering the member ID may result in configuration change or loss. Continue? [Y/N]:y
[Sysname] quit
<Sysname> reboot
```

\# 根据[图1-15](https://cdn.jsdelivr.net/gh/xyzz-by/imge@main/2021/20220427203415.png)选定IRF物理端口并进行物理连线。

\# 重新登录到设备，关闭选定的所有IRF物理端口。

```
<Sysname> system-view
[Sysname] interface range ten-gigabitethernet 4/0/49 to ten-gigabitethernet 4/0/52
[Sysname-if-range] shutdown
[Sysname-if-range] quit
```

\# 配置IRF端口4/1，并将它与物理端口Ten-GigabitEthernet4/0/49和Ten-GigabitEthernet4/0/50绑定。

```
[Sysname] irf-port 4/1
[Sysname-irf-port4/1] port group interface ten-gigabitethernet 4/0/49
[Sysname-irf-port4/1] port group interface ten-gigabitethernet 4/0/50
[Sysname-irf-port4/1] quit
```

\# 配置IRF端口4/2，并将它与物理端口Ten-GigabitEthernet4/0/51和Ten-GigabitEthernet4/0/52绑定。

```
[Sysname] irf-port 4/2
[Sysname-irf-port4/2] port group interface ten-gigabitethernet 4/0/51
[Sysname-irf-port4/2] port group interface ten-gigabitethernet 4/0/52
[Sysname-irf-port4/2] quit
```

\# 开启Ten-GigabitEthernet4/0/49～Ten-GigabitEthernet4/0/52端口，并保存配置。

```
[Sysname] interface range ten-gigabitethernet 4/0/49 to ten-gigabitethernet 4/0/52
[Sysname-if-range] undo shutdown
[Sysname-if-range] quit
[Sysname] save
```

\# 激活IRF端口下的配置。

```
[Sysname] irf-port-configuration active
```

(7) Device D将自动重启，加入Device A、Device B和Device C已经形成的IRF。

#### (8) 配置BFD MAD

**VLAN方式**

\# 创建VLAN 3，并将端口GigabitEthernet1/0/1、GigabitEthernet2/0/1、GigabitEthernet3/0/1和GigabitEthernet4/0/1加入VLAN 3中。 # 此VLAN只能作为BFD MAD检测用，不要用于其他业务。

**如设备配置多条检测链路，只需要将对应端口加入到检测VLAN里即可，并关闭端口的STP。并且不配置链路聚合**

```
[Sysname] vlan 3
[Sysname-vlan3] port gigabitethernet 1/0/1 gigabitethernet 2/0/1 gigabitethernet 3/0/1 gigabitethernet 4/0/1
[Sysname-vlan3] quit
```

\# 创建VLAN接口3，并配置MAD IP地址。

```
[Sysname] interface vlan-interface 3
[Sysname-Vlan-interface3] mad bfd enable
[Sysname-Vlan-interface3] mad ip address 192.168.2.1 24 member 1
[Sysname-Vlan-interface3] mad ip address 192.168.2.2 24 member 2
[Sysname-Vlan-interface3] mad ip address 192.168.2.3 24 member 3
[Sysname-Vlan-interface3] mad ip address 192.168.2.4 24 member 4
[Sysname-Vlan-interface3] quit
```

\# 因为BFD MAD和生成树功能互斥，所以在GigabitEthernet1/0/1、GigabitEthernet2/0/1、GigabitEthernet3/0/1和GigabitEthernet4/0/1端口上关闭生成树协议。

```
[Sysname] interface range gigabitethernet 1/0/1 gigabitethernet 2/0/1 gigabitethernet 3/0/1 gigabitethernet 4/0/1
[Sysname-if-range] undo stp enable
[Sysname-if-range] quit
```

(9) 配置中间设备Device E

Device E作为中间设备来透传BFD MAD报文，协助IRF中的四台成员设备进行多Active检测。

!\[注意]如果中间设备是一个IRF系统，则必须通过配置确保其IRF域编号与被检测的IRF系统不同。

\# 创建VLAN 3，并将端口GigabitEthernet1/0/1～GigabitEthernet1/0/4加入VLAN 3中，用于转发BFD MAD报文。

```
<DeviceE> system-view
[DeviceE] vlan 3
[DeviceE-vlan3] port gigabitethernet 1/0/1 to gigabitethernet 1/0/4
[DeviceE-vlan3] quit
```

**三层聚合方式**

举例：两个设备

\# 创建三层聚合口

```
 [Sysname]interface Route-Aggregation 1
```

\# 创建三层聚合接口1，并配置MAD IP地址。

```
[Sysname] interface Route-Aggregation 1
[Sysname] mad bfd enable
[Sysname] mad ip address 192.168.1.1 255.255.255.252 member 1
[Sysname] mad ip address 192.168.1.2 255.255.255.252 member 2
[Sysname] quit
```

\# 将端口GigabitEthernet1/0/1、GigabitEthernet2/0/1加入到三层聚合口内。

**如有配置多条检测链路，只需要将对应的端口加入到聚合组即可**

```
[Sysname] interface GigabitEthernet1/0/1
[Sysname] port link-mode route
[Sysname] port link-aggregation group 1
[Sysname] quit
[Sysname] interface GigabitEthernet1/0/2
[Sysname] port link-mode route
[Sysname] port link-aggregation group 1
[Sysname] quit
```

#### 检测验证

```
[Sysname]display mad verbose
Multi-active recovery state: No
Excluded ports (user-configured):
Excluded ports (system-configured):
  Ten-GigabitEthernet1/0/49
  Ten-GigabitEthernet1/0/50
  Ten-GigabitEthernet2/0/49
  Ten-GigabitEthernet2/0/50
MAD ARP disabled.
MAD ND disabled.
MAD LACP disabled.
MAD BFD enabled interface: Route-Aggregation1
  MAD status                 : Normal
  Member ID   MAD IP address       Neighbor   MAD status
  1           1.1.1.1/30           2          Normal
  2           1.1.1.2/30           1          Normal
```

测试拔掉堆叠线，Standby 设备上端口全部Down，说明正常

## 其他配置

### 1.IRF显示和维护

| 操作                    | 命令                               |
| --------------------- | -------------------------------- |
| 显示IRF中所有成员设备的相关信息     | **display irf**                  |
| 显示IRF的拓扑信息            | **display irf topology**         |
| 显示IRF链路信息             | **display irf link**             |
| 显示所有成员设备上重启以后生效的IRF配置 | **display irf configuration**    |
| 显示MAD配置信息             | **display mad** \[ **verbose** ] |

### 2.配置成员优先级

在主设备选举过程中，优先级数值大的成员设备将优先被选举成为主设备。

IRF形成后，也可以通过本配置修改成员优先级，但修改不会触发选举，修改的优先级在下一次选举时生效。

#### 2.1. 配置步骤

(1) 配置IRF中指定成员设备的优先级。

```
[Sysname] irf member 1 priority 32
```

缺省情况下，设备的成员优先级为1。数值越大优先级越高，最大32。

### 3.IRF链路状态变化延迟上报功能

该功能用于避免因端口链路层状态在短时间内频繁改变，导致IRF分裂/合并的频繁发生。

#### 3.1. 配置限制和指导

下列情况下，建议将IRF链路状态变化延迟上报时间配置为0：

· 对主备倒换速度和IRF链路切换速度要求较高时

· 在IRF环境中使用RRPP、BFD或GR功能时

· 在执行关闭IRF物理端口或重启IRF成员设备的操作之前，请首先将IRF链路状态变化延迟上报时间配置为0，待操作完成后再将其恢复为之前的值

#### 3.2. 配置步骤

配置IRF链路状态变化延迟上报时间。

```
 [Sysname] irf link-delay 0
```

一般实际环境两台设备直链，建议配置成0秒，默认IRF链路状态变化延迟上报时间为4秒。

### 4.配置保留接口

当IRF分裂时，会关闭Recovery状态IRF上除了系统保留接口外的所有业务接口。系统保留接口包括：

· IRF物理端口， BFD MAD检测接口， 用户配置的保留聚合接口的成员接口

如果接口有特殊用途需要保持up状态（比如Telnet登录接口等），则用户可以通过命令行将这些接口配置为保留接口。

#### 1. 配置限制和指导

使用VLAN接口进行远程登录时，需要将该VLAN接口及其对应的以太网端口都配置为保留接口。但如果在正常工作状态的IRF中该VLAN接口也处于UP状态，则在网络中会产生IP地址冲突。

#### 2. 配置步骤

(1) 配置保留接口，当设备进入Recovery状态时，该接口不会被关闭。

```
[Sysname] mad exclude interface GigabitEthernet 1/0/1
```

缺省情况下，设备进入Recovery状态时会自动关闭本设备上除了系统保留接口以外的所有业务接口。

### 扩展

#### 角色选举

确定成员设备角色为主设备或从设备的过程称为角色选举。角色选举会在以下情况下进行：IRF建立、主设备离开或者故障、IRF合并等。其中，IRF合并包括合并前独立运行的两个（或多个）IRF合并为一个IRF和IRF分裂后重新合并两种情况。

IRF建立、主设备离开或者故障、独立运行的两个（或多个）IRF合并为一个IRF时，角色选举规则如下：

(1)     当前主设备优先，IRF不会因为有新的成员设备/主控板加入而重新选举主设备。不过，当IRF形成时，因为没有主设备，所有加入的设备都认为自己是主设备，则继续下一条规则的比较。

(2)     成员优先级大的优先。如果优先级相同，则继续下一条规则的比较。

(3)     系统运行时间长的优先。在IRF中，成员设备启动时间间隔精度为10分钟，即10分钟之内启动的设备，则认为它们是同时启动的，则继续下一条规则的比较。

(4)     CPU MAC小的优先。

通过以上规则选出的最优成员设备即为主设备，其它成员设备则均为从设备。

IRF分裂后重新合并时，原Recovery状态IRF中所有成员设备自动重启以从设备身份加入原正常工作状态的IRF，原正常工作状态的IRF的主设备作为合并后IRF的主设备。

在角色选举完成后，IRF形成，进入IRF管理与维护阶段。









引用：

{% embed url="https://www.h3c.com/cn/d_202203/1575362_30005_0.htm#_Toc98968919" %}
IRF虚拟化技术配置指导
{% endembed %}

{% embed url="https://www.h3c.com/cn/d_202010/1348507_30005_0.htm" %}
IRF虚拟化技术配置举例
{% endembed %}

