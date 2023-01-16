# 常用命令

### 常用的查看命令

```
1)Ruijie#show running-config	//查看设备当前的配置
2)Ruijie#show version	//查看设备的软件版本信息
3)Ruijie#show log	//查看设备的日志
4)Ruijie#show ip interface brief   //查看设备的三层接口ip和掩码
5)Ruijie#show interface f0/1	//查看f0/1接口的信息，包括mac地址，流量信息等等
6)Ruijie#show interface status	//查看交换接口的信息,包括状态、vlan、双工、速度和使用介质
7)Ruijie#show vlan	//查看交换接口所属的vlan信息
8)Ruijie#show mac-address-table	//查看交换机当前MAC地址表信息
9)Ruijie#show ip interface brief	//查看接口IP地址
10)Ruijie#show vlan		//查看所有VLAN信息 
11)Ruijie#show aggregateport 1 summary    查看聚合端口AG1的信息  
12)Ruijie#show spanning-tree        //查看生成树配置信息
13)Ruijie#show spanning-tree interface fastethernet 0/1  //查看该端口的生成树状态
14)Ruijie#show port-security         //查看交换机的端口安全配置信息
15)Ruijie#show port-security address   //查看地址安全绑定配置信息
16)Ruijie#show ip access-lists listname  //查看名为listname的列表的配置信息
17)Ruijie#
18)Ruijie#
19)Ruijie#
```

### 交换机命名

```
RuiJie>enable
RuiJie# Configure terminal  
RuiJie(config)# hostname SwitchA 
SwitchA(config)# 
```

### 端口配置

```
RuiJie(config)#interface gigabitEthernet 0/3  //进入g0/3的端口配置模式
RuiJie(config)#interface range gigabitEthernet 0/1-2,0/5,0/7-9  //进入g0/1、g0/2、g0/5、g0/7、g0/8、g0/9的端口配置模式
RuiJie(config-if)#speed 10    //配置端口速率为10M
RuiJie(config-if)#duplex full     //配置端口为全双工模式
RuiJieRuiJie(config-if)#no shutdown       //开启该端口，一般默认开启


1.1）设置端口类型为access模式并放通vlan10通过
RuiJie(config-if)#sswitchport mode access	//将该端口设为access模式
RuiJie(config-if)#switchport access vlan 10  //将该端口划入VLAN10
1.2）设置端口类型为trunk模式并放通vlan11到vlan19通过
RuiJie(config-if)#switchport mode trunk     //将该端口设为trunk模式
RuiJie(config-if)#switchport trunk allowed vlan only 11-19 //trunk模式,放通vlan 11-19
1.3）设置端口类型为trunk模式,设置Untagged为vlan10
RuiJie(config-if)#switchport trunk native vlan 10 //设置Untagged为vlan 10，进入交换机的数据打上vlan10的标签。类似于华三的pvid

RuiJie(config-if)#port-group 1 		//将该端口划入聚合端口AG1中,用于聚合端口 

Ruijie(config)#int g0/24        //进入G0/24口
Ruijie(config-if-GigabitEthernet 0/24)#no switchport    //将接口更改为路由口
Ruijie(config-if-GigabitEthernet 0/24)#ip add 10.10.10.2 255.255.255.0   //配置IP 10.10.10.2/24
```

### 远程登录

```
1)全局开启SSH服务，并制定SSH的版本
Ruijie>enable 
Ruijie#configure terminal
Ruijie(config)#enable service ssh-server   //开启SSH服务，默认关闭
Ruijie(config)#ip ssh version 2               //默认1.99版本，设置为版本2

2)添加登陆的用户名和密码
Ruijie(config)#username ruijie password ruijie    //用户名ruijie密码ruijie

3）在VTY线程下调用
Ruijie(config)#lin vty 0 4
Ruijie(config-line)#password ruijie    //设置登陆密码为ruijie
Ruijie(config-line)# login   local
Ruijie(config-line)#transport input ssh     //设置传输模式是SSH，设置远程登入只允许使用SSH登入，不能使用telnet登入

4）保存配置
Ruijie(config-line)#end          //退出到特权模式
Ruijie#write                   //确认配置正确，保存配置
```

### 创建链路聚合口

```
RuiJie>enable
RuiJie# Configure terminal  
RuiJie#(config)# interface aggregateport 1   //创建聚合接口AG1
RuiJie#(config-if)# switchport mode trunk   //配置并保证AG1为 trunk 模式
RuiJie#(config)#int g0/23-24             //进入端口G0/23和G0/24
RuiJie#(config-if-range)#port-group 1       //将端口（端口组）划入聚合端口AG1中
```

### 配置路由

```
Ruijie(config)# ip  route 192.168.0.0 255.255.0.0 10.10.10.2
192.168.0.0 255.255.0.0 目的网段  
10.10.10.2 下一跳
```

### DHCP服务

```
Ruijie(config)#service dhcp         //启用DHCP服务
Ruijie(config)#int vlan 40     //VLAN创建对应的三层接口
Ruijie(config-if-VLAN 40)#ip add  192.168.40.1  255.255.255.0
Ruijie(config)#ip dhcp pool youxian        //创建DHCP地址池，名字为youxian
Ruijie(dhcp-config)#network 192.168.40.0 255.255.255.0     //为有线用户动态分配192.168.40.0/24网段的地址
Ruijie(dhcp-config)#dns-server 114.114.114.114 8.8.8.8      //为有线用户分配2个DNS地址，分别为114.114.114.114和8.8.8.8
Ruijie(dhcp-config)#default-router 192.168.40.1     //有线用户的网关地址为192.168.40.1
Ruijie(dhcp-config)#exit
```
