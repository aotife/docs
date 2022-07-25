# IP Source Guard与DHCP Snooping配合的IPv4动态绑定功能配置举例

标签：

* DHCP防止用户修改IP的方法
* 禁止手动设置IP地址

## 1. 组网需求

DHCP客户端通过Device的接口Ten-GigabitEthernet1/1/1接入网络，通过DHCP服务器获取IPv4地址。

具体应用需求如下：

· Device上使能DHCP Snooping功能，保证客户端从合法的服务器获取IP地址，且记录客户端IPv4地址及MAC地址的绑定关系。

· 在接口Ten-GigabitEthernet1/1/1上启用IPv4动态绑定功能，利用动态生成的DHCP Snooping表项过滤接口接收的报文，只允许通过DHCP服务器动态获取IP地址的客户端接入网络。DHCP服务器的具体配置请参见“三层技术-IP业务配置指导”中的“DHCP服务器”。

## 2. 组网图

图1-3 配置与DHCP Snooping配合的IPv4动态绑定功能组网图

![img](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220527111729.png)

## 3. 配置步骤

(1) 配置DHCP Snooping

\# 配置各接口的IP地址（略）。

\# 开启DHCP Snooping功能。

```
<Device> system-view

[Device] dhcp snooping enable
```

\# 设置与DHCP服务器相连的接口Ten-GigabitEthernet1/1/2为信任接口。

```
[Device] interface ten-gigabitethernet 1/1/2

[Device-Ten-GigabitEthernet1/1/2] dhcp snooping trust

[Device-Ten-GigabitEthernet1/1/2] quit
```

(2) 配置IPv4接口绑定功能

\# 配置接口Ten-GigabitEthernet1/1/1的IPv4接口绑定功能，绑定源IP地址和MAC地址，并启用接口的DHCP Snooping 表项记录功能。

```
[Device] interface ten-gigabitethernet 1/1/1

[Device-Ten-GigabitEthernet1/1/1] ip verify source ip-address mac-address

[Device-Ten-GigabitEthernet1/1/1] dhcp snooping binding record

[Device-Ten-GigabitEthernet1/1/1] quit
```

## 4. 验证配置

\# 显示接口Ten-GigabitEthernet1/1/1从DHCP Snooping获取的动态表项。

```
[Device] display ip source binding dhcp-snooping

Total entries found: 1

IP Address   MAC Address  Interface        VLAN Type

192.168.0.1   0001-0203-0406 XGE1/1/1         1  DHCP snooping
```

从以上显示信息可以看出，接口Ten-GigabitEthernet1/1/1在配置IPv4接口绑定功能之后根据DHCP Snooping表项产生了动态绑定表项。

引用：

* https://www.h3c.com/cn/d\_202002/1274388\_30005\_0.htm#\_Toc32417057
