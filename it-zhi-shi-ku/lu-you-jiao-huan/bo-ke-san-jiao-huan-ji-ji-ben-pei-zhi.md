# 博科SAN交换机基本配置

默认管理地址：

```
10.77.77.77
```

账号/密码

```
admin/password
```

串口参数(Console)

```
Bits per second: 9600
Databits: 8
Parity: none
Stop bits: 1
Flow control: none
```

## 配置WWN-Zone

### 1.所有的设备上线开机，连接交换机

### 2.查看端口识别到的wwn

### `switchshow`

```
swd77:admin> switchshow 
switchName:     swd77 
switchType:     71.2 
switchState:    Online 
switchMode:     Native 
switchRole:     Principal 
switchDomain:   1 
switchId:       fffc01 
switchWwn:      10:00:50:eb:1a:3f:0b:98 
zoning:         ON (newconfig) 
switchBeacon:   OFF
​
Index Port Address Media Speed State     Proto 
============================================== 
  0   0   010000   id    N8   Online      FC  F-Port  50:05:07:68:03:09:fb:d8 
  1   1   010100   id    N8   Online      FC  F-Port  50:05:07:68:03:09:fb:d9 
  2   2   010200   id    N8   Online      FC  F-Port  21:00:00:24:ff:0e:19:66 
  3   3   010300   id    N8   Online      FC  F-Port  21:00:00:24:ff:65:e2:49 
  4   4   010400   id    N8   Online      FC  F-Port  10:00:00:05:1e:fd:98:8c 
  5   5   010500   id    N8   Online      FC  F-Port  10:00:00:05:1e:fd:97:48 
  6   6   010600   id    N8   No_Light    FC 
  7   7   010700   id    N8   No_Light    FC 
  8   8   010800    –    N8   No_Module   FC  (No POD License) Disabled 
  9   9   010900    –    N8   No_Module   FC  (No POD License) Disabled 
 10  10   010a00    –    N8   No_Module   FC  (No POD License) Disabled 
 11  11   010b00    –    N8   No_Module   FC  (No POD License) Disabled 
 12  12   010c00    –    N8   No_Module   FC  (No POD License) Disabled 
 13  13   010d00    –    N8   No_Module   FC  (No POD License) Disabled 
 14  14   010e00    –    N8   No_Module   FC  (No POD License) Disabled 
 15  15   010f00    –    N8   No_Module   FC  (No POD License) Disabled 
 16  16   011000    –    N8   No_Module   FC  (No POD License) Disabled 
 17  17   011100    –    N8   No_Module   FC  (No POD License) Disabled 
 18  18   011200    –    N8   No_Module   FC  (No POD License) Disabled 
 19  19   011300    –    N8   No_Module   FC  (No POD License) Disabled 
 20  20   011400    –    N8   No_Module   FC  (No POD License) Disabled 
 21  21   011500    –    N8   No_Module   FC  (No POD License) Disabled 
 22  22   011600    –    N8   No_Module   FC  (No POD License) Disabled 
 23  23   011700    –    N8   No_Module   FC  (No POD License) Disabled
```

### 3.创建服务器wwn的别名

```
alicreate "esx1_HBA1","50:05:07:68:03:09:fb:d8"
alicreate "esx1_HBA2","50:05:07:68:03:09:fb:d9"
alicreate "esx2_HBA1","21:00:00:24:ff:0e:19:66"
alicreate "esx2_HBA2","21:00:00:24:ff:65:e2:49"
```

### 4.创建存储wwn的别名

```
alicreate "Unity_spa01","10:00:00:05:1e:fd:98:8c"
alicreate "Unity_spb01","10:00:00:05:1e:fd:97:48"
```

### 5.创建zone

```
zonecreate "esx1_HBA1_Unity_spa01","esx1_HBA1;Unity_spa01"
zonecreate "esx1_HBA2_Unity_spa01","esx1_HBA2;Unity_spa01"
zonecreate "esx1_HBA1_Unity_spb01","esx1_HBA1;Unity_spb01"
zonecreate "esx1_HBA2_Unity_spb01","esx1_HBA2;Unity_spb01"
zonecreate "esx2_HBA1_Unity_spa01","esx2_HBA1;Unity_spa01"
zonecreate "esx2_HBA2_Unity_spa01","esx2_HBA2;Unity_spa01"
zonecreate "esx2_HBA1_Unity_spb01","esx2_HBA1;Unity_spb01"
zonecreate "esx2_HBA2_Unity_spb01","esx2_HBA2;Unity_spb01"
```

### 6.保存zone

```
cfgcreate "cfg","esx1_HBA1_Unity_spa01"  //创建配置文件cfg，并添加"esx1_HBA1_Unity_spa01"zone
cfgadd "cfg","esx1_HBA2_Unity_spa01"     //添加"esx1_HBA2_Unity_spa01"zone到cfg配置文件
cfgadd "cfg","esx1_HBA1_Unity_spb01"
cfgadd "cfg","esx1_HBA2_Unity_spb01"
cfgadd "cfg","esx2_HBA1_Unity_spa01"
cfgadd "cfg","esx2_HBA2_Unity_spa01"
cfgadd "cfg","esx2_HBA1_Unity_spb01"
cfgadd "cfg","esx2_HBA2_Unity_spb01"
```

### 7.启用配置文件

```
cfgenable "cfg"   输入y
```

### 8.保存配置

```
cfgsave   输入y
```

## 配置端口-Zone

### 1.创建zone

zonecreate "Zone名称","端口;或者别名"

```
zonecreate "esx1_HBA1_Unity_spa01","1,0;1,4"
zonecreate "esx1_HBA2_Unity_spa01","1,1;1,4"
zonecreate "esx1_HBA1_Unity_spb01","1,0;1,5"
zonecreate "esx1_HBA2_Unity_spb01","1,1;1,5"
zonecreate "esx2_HBA1_Unity_spa01","1,2;1,4"
zonecreate "esx2_HBA2_Unity_spa01","1,3;1,4"
zonecreate "esx2_HBA1_Unity_spb01","1,2;1,5"
zonecreate "esx2_HBA2_Unity_spb01","1,3;1,5"
```

### 2.保存zone

```
cfgcreate "cfg","esx1_HBA1_Unity_spa01"  //创建配置文件cfg，并添加"esx1_HBA1_Unity_spa01"zone
cfgadd "cfg","esx1_HBA2_Unity_spa01"     //添加"esx1_HBA2_Unity_spa01"zone到cfg配置文件
cfgadd "cfg","esx1_HBA1_Unity_spb01"
cfgadd "cfg","esx1_HBA2_Unity_spb01"
cfgadd "cfg","esx2_HBA1_Unity_spa01"
cfgadd "cfg","esx2_HBA2_Unity_spa01"
cfgadd "cfg","esx2_HBA1_Unity_spb01"
cfgadd "cfg","esx2_HBA2_Unity_spb01"
```

### 3.启用配置文件

```
cfgenable "cfg"   输入y
```

### 4.保存配置

```
cfgsave   输入y
```

## 常用命令

### 1.更改管理地址

管理地址 192.168.1.1

掩码：255.255.255.0

网关：192.168.1.254

```
Brocade:admin>ipaddrset
    Ethernet IP Address [10.77.77.77]: 192.168.1.1
    Ethernet Subnetmask [0.0.0.0]: 255.255.255.0
    Fibre Channel IP Address [none]:
    Fibre Channel Subnetmask [none]:
    Gateway Address [172.17.1.1]:192.168.1.254
    Set IP address now? [y = set now, n = next reboot]: y
```

### 2.修改密码

```
Brocade:admin>passwd admin
```

### 3.查看当前管理地址

```
ipaddrshow
```

### 4.查看版本

```
version
```

### 5.查看wwn

```
switchshow
```

### 6.查看zone

```
zoneshow
```

### 7.查看配置文件

```
cfgshow
```

### 8.查看光功率

一般:光衰值，RX/TX,同机房4dm，异地机房光纤在7dm

```
sfpshow 1 -f    //查看1口的光功率
```

```
sfpshow -all | grep -E "RX|TX|Port"   //全部端口
```

### 9. 设置日期和时间

设置命令：

```
Date “mmddHHMMyy”
```

**参数描述：**

mm:： 代表月份、设置值为01到12

dd：代表日期，设置值为01到31

hh：代表时间，设置值从00到23

MM：代表分钟，设置值从00到59

yy：代表年，设置值从00到99（大于69代表1970-1999，小于70代表2000-2069）

查看时间

```
switch:admin> date
​
Fri Jan 29 17:01:48 UTC 2000
​
switch:admin> date "0227123003"
​
Thu Feb 27 12:30:00 UTC 200
​
switch:admin>
​
​
```

### 10.修改zone

#### 10.1、删除别名

```
\> alidelete “aliname”  
​
如：
​
\> alidelete “ds4700_A”
```

#### 10.2、删除zone

```
\> zonedelete “zonename”  
​
如：
​
\> zonedelete “Zone_yxdb1”
```

#### 10.3、删除config

```
> cfgdelete “cfgname”   
​
如：
​
\> cfgdelete “Cfg_gztk”
```

#### 10.4、增加成员到已存在的alias

```
> aliadd “aliname”,”member;member” //添加端口或wwn到别名
​
如：
​
\> aliadd "hr_host1","1,5"    
```

#### 10.5、增加成员到已存在的zone

```
>zoneadd “zonename”,”mumber;mumber” //添加别名到zone
​
如：
​
\> zoneadd "Zone_hr","hr_host1"   
```

#### 10.6、增加成员到已存在的config

```
\>cfgadd “cfgname”,”mumber;mumber”  // 添加zone到config
​
如：
​
\> cfgadd "Cfg_gztk","Zone_hr"   
```

#### 10.7、从config移除成员

```
\> cfgremove “cfgname”, ”mumber;mumber”  // 从config移除zone
​
如：
​
\> cfgremove "Cfg_gztk","Zone_hr"   
​
如果config里所有的zone都被移除了那么该config将被删除
```

#### 10.8、从zone移除成员

```
\> zoneremove “cfgname”, ”mumber;mumber”  // 从zone移除alias
​
如：
​
\> zoneremove "Zone_hr ","DS4700_A"  
​
如果Zone里所有的alias都被移除了那么该zone将被删除 
```

#### 10.9、从alias移除成员

```
\> aliremove “aliname”, ”mumber;mumber”  // 从alias移除端口或wwn
​
如：
​
\> aliremove "DS4700_A","1,3" 
​
如果alias里所有的端口或wwn都被移除了那么该alias将被删除  
```

### 删除交换机的配置

\> cfgdisable //删除之前先禁用配置

\> cfgclear //清空所有的配置、zone和别名

\> cfgsave //保存

### .其他维护命令

`uptime` 显示交换机工作时间

`ipaddrshow` 显示交换机IP地址信息

`licenseshow` 显示当前交换机所添加的license信息

`switchshow` 检查交换机信息及端口状态

`switchstatusshow` 显示交换机的运行状态

`firmwareshow` 显示微码版本

`fanshow` 显示风扇运行状态

`tempshow` 显示交换机当前温度信息

`psshow` 显示电源运行状态

`slotshow` 显示板卡运行状态

`hashshow` 显示CP版卡HA状态,

`errdump` 显示错误日志

`fabricshow` 显示fabric信息及级联信息

`cfgshow` 显示交换机配置信息

`porterrshow` 显示交换机端口的错误统计

`chassisshow` 显示外壳信息及序列号

`userconfig --show -a` 查看登录帐号

`domainsshow` 查看交换机的domain信息

`aliashow` 查看别名

***

**级联ping**

级联互ping:`fcping --number 10000 --length 2036 --interval 1 10:00:00:05:33:9f:06:28`

级联更改`portcfglongdistance 0`

***

**备份**

```
configuplocad
​
Protocol (scp or ftp):ftp
​
host:
​
user name:
​
filename:
​
password:
```

***

**恢复**

```
switchdisable  停用交换机
​
configdownload
​
Protocol (scp or ftp):ftp
​
Server Name or IP Address [host]:
​
User Name [user]:
​
File Name [config.txt]:config.txt
​
.........
​
Do you want to continue [y/n]:y
​
Password:
```

恢复出厂设置

```
switchdisable -----停用交换机
​
cfgdisable（按y或者yes）-----禁用区域配置
​
cfgclear（按y或者yes）-----删除所有信息
​
cfgsave（按y或者yes）-----保存配置
​
configdefault -----恢复默认配置
​
switchenable -----重新启用交换机
​
reboot（重启就可以了）
```

***

**端口错误统计信息**

**`porterrshow`**

```
观察报错，主要看enc-out，crc-err是否报错，c3是否有丢包
​
enc-out：单独报错，是光纤线的问题
​
crc-err：数据帧crc效验错误，重启服务器，拔插过线都会有报错，需清除过10分钟后再观察是否有报错
​
disc c3：是否丢包
​
statsclear 清除报错
```

\
