# 华为交换机开启远程Telnet登录

## 配置登录地址

```
[Switch] vlan 10
[Switch-vlan10]  interface vlanif 10    //配置VLANIF10作为管理接口。
[Switch-Vlanif10] ip address 10.1.1.1 24
[Switch-Vlanif10] quit
[Switch] interface gigabitethernet 0/0/10    
[Switch-GigabitEthernet0/0/10] port link-type access    //配置接口类型为access。
[Switch-GigabitEthernet0/0/10] port default vlan 10    //配置接口GE0/0/10加入VLAN 10。
[Switch-GigabitEthernet0/0/10] quit
```

## 配置Telnet

```
[Switch] telnet server enable    //使能Telnet功能。
[Switch] telnet server-source -i Vlanif 10    //配置服务器端的源接口为10.1.1.1对应的接口，假设该接口为Vlanif 10。
[Switch] user-interface vty 0 4    //进入VTY 0～VTY 4用户界面视图。
[Switch-ui-vty0-4] protocol inbound telnet    //指定VTY用户界面所支持的协议为Telnet。
[Switch-ui-vty0-4] user privilege level 15    //配置VTY 0～VTY 4的用户级别为15级。
[Switch-ui-vty0-4] authentication-mode aaa    //配置VTY 0～VTY 4的用户认证方式为AAA认证。
[Switch-ui-vty0-4] quit
[Switch] aaa
[Switch-aaa] local-user admin123 password irreversible-cipher Huawei@6789    //创建名为admin123的本地用户，设置其登录密码为Huawei@6789。V200R003之前的版本，不支持irreversible-cipher，仅支持cipher关键字。
[Switch-aaa] local-user admin123 privilege level 15    //配置用户级别为15级。
Warning: This operation may affect online users, are you sure to change the user privilege level ?[Y/N]y   
[Switch-aaa] local-user admin123 service-type telnet    //配置接入类型为telnet，即：Telnet用户。
[Switch-aaa] quit
```

## 配置文件

Switch的配置文件

```
#
sysname Switch
#
vlan batch 10
#
telnet server enable
telnet server-source -i Vlanif 10
#
clock timezone BJ add 08:00:00
#
aaa
 local-user admin123 password irreversible-cipher %^%#}+ysUO*B&+p'NRQR0{ZW7[GA*Z*!X@o:Va15dxQAj+,$>NP>63de|G~ws,9G%^%#
 local-user admin123 privilege level 15
 local-user admin123 service-type telnet
 local-user admin1234 password irreversible-cipher %^%#}+ysUO*B&+p'NRQR0{ZW7[GA*Z*!X@o:Va15dxQAj+,$>NP>63de|G~ws,9G%^%#
 local-user admin1234 privilege level 15
 local-user admin1234 service-type terminal
#
interface Vlanif10
 ip address 10.1.1.1 255.255.255.0
#
interface GigabitEthernet0/0/10
 port link-type access
 port default vlan 10
#
user-interface con 0
 authentication-mode aaa
user-interface vty 0 4
 authentication-mode aaa
 user privilege level 15 
 protocol inbound telnet
#
return
```

## 脚本

```
telnet server enable
telnet server-source -i all
user-interface vty 0 4
protocol inbound telnet
user privilege level 15 
authentication-mode aaa
quit
aaa
local-user admin password irreversible-cipher Huawei@123
local-user admin123 privilege level 15 
local-user admin123 service-type telnet
quit

```



引用：

{% embed url="https://support.huawei.com/enterprise/zh/doc/EDOC1000069491/176653a6" %}
