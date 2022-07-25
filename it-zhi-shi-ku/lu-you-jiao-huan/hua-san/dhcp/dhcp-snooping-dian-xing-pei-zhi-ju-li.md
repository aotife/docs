---
description: DHCP信任端口
---

# DHCP Snooping典型配置举例

## DHCP Snooping典型配置

标签：

* \#DHCP信任端口

## 1.全局开启DHCP Snooping配置举例

#### 1.1组网需求

Switch B通过以太网端口Ten-GigabitEthernet1/0/1连接到合法DHCP服务器，通过以太网端口Ten-GigabitEthernet1/0/3连接到非法DHCP服务器，通过Ten-GigabitEthernet1/0/2连接到DHCP客户端。要求：

· 与合法DHCP服务器相连的端口可以转发DHCP服务器的响应报文，而其他端口不转发DHCP服务器的响应报文。

· 记录DHCP-REQUEST报文和信任端口收到的DHCP-ACK报文中DHCP客户端IP地址及MAC地址的绑定信息。

#### 1.2 组网图

图1-3 DHCP Snooping组网示意图

![img](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220527113632.png)

‌

#### 1.3. 配置步骤

\# 全局开启DHCP Snooping功能。

```
<SwitchB> system-view

[SwitchB] dhcp snooping enable
```

\# 设置Ten-GigabitEthernet1/0/1端口为信任端口。

```
[SwitchB] interface ten-gigabitethernet 1/0/1

[SwitchB-Ten-GigabitEthernet1/0/1] dhcp snooping trust

[SwitchB-Ten-GigabitEthernet1/0/1] quit
```

\# 在Ten-GigabitEthernet1/0/2上开启DHCP Snooping表项功能。

```
[SwitchB] interface ten-gigabitethernet 1/0/2

[SwitchB-Ten-GigabitEthernet1/0/2] dhcp snooping binding record

[SwitchB-Ten-GigabitEthernet1/0/2] quit
```

#### 1.4. 验证配置

配置完成后，DHCP客户端只能从合法DHCP服务器获取IP地址和其它配置信息，非法DHCP服务器无法为DHCP客户端分配IP地址和其他配置信息。且使用**display dhcp snooping binding**可查询到获取到的DHCP Snooping表项。

### 2. 按VLAN开启DHCP Snooping配置举例

#### 1. 组网需求

Switch B通过以太网端口Ten-GigabitEthernet1/0/1连接到合法DHCP服务器，通过以太网端口Ten-GigabitEthernet1/0/3连接到非法DHCP服务器，通过Ten-GigabitEthernet1/0/2连接到DHCP客户端。要求：

· VLAN 100上与合法DHCP服务器相连的端口可以转发DHCP服务器的响应报文，而其他端口不转发DHCP服务器的响应报文。

· 记录DHCP-REQUEST报文和信任端口收到的DHCP-ACK报文中DHCP客户端IP地址及MAC地址的绑定信息。

#### 2. 组网图

图1-4 按VLAN开启DHCP Snooping配置组网示意图

![img](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220527113638.png)

‌

#### 3. 配置步骤

\# 配置端口Ten-GigabitEthernet1/0/1、Ten-GigabitEthernet1/0/2和Ten-GigabitEthernet1/0/3为Access端口，允许VLAN 100通过。

```
<SwitchB> system-view

[SwitchB] vlan 100

[SwitchB-vlan100] port ten-gigabitethernet 1/0/1 to ten-gigabitethernet 1/0/3

[SwitchB-vlan100] quit
```

\# 在VLAN100内开启DHCP Snooping功能。

```
[SwitchB] dhcp snooping enable vlan 100
```

\# 指定端口Ten-GigabitEthernet1/0/1为VLAN 100下DHCP Snooping功能的信任端口。

```
[SwitchB] vlan 100

[SwitchB-vlan100] dhcp snooping trust interface ten-gigabitethernet 1/0/1
```

\# 在VLAN 100内开启DHCP Snooping表项记录功能。

```
[SwitchB-vlan100] dhcp snooping binding record

[SwitchB-vlan100] quit
```

#### 4. 验证配置

配置完成后，DHCP客户端只能从合法DHCP服务器获取IP地址和其它配置信息，非法DHCP服务器无法为DHCP客户端分配IP地址和其他配置信息。且使用\*\*`display dhcp snooping binding`\*\*可查询到获取到的DHCP Snooping表项。

引用：

http://www.h3c.com/cn/d\_202102/1382381\_30005\_0.htm#\_Toc63109203
