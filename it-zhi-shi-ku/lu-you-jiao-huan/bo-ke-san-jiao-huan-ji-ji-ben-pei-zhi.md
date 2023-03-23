# 博科SAN交换机基本配置

默认管理地址：

```
10.77.77.77
```

账号/密码

```
admin/password
```

## 配置WWN-Zone

### 1.所有的设备上线，连接交换机

### 2.查看端口识别到的wwn，`switchshow`

```
swd77:admin> switchshow 
switchName:     swd77 
switchType:     71.2 
switchState:    Online 
switchMode:     Native 
switchRole:     Principal 
switchDomain:   1 
switchId:       fffc01 
switchWwn:      10:00:50:eb:1a:3f:0b:98 
zoning:         ON (newconfig) 
switchBeacon:   OFF

Index Port Address Media Speed State     Proto 
============================================== 
  0   0   010000   id    N8   Online      FC  F-Port  50:05:07:68:03:09:fb:d8 
  1   1   010100   id    N8   Online      FC  F-Port  50:05:07:68:03:09:fb:d9 
  2   2   010200   id    N8   Online      FC  F-Port  21:00:00:24:ff:0e:19:66 
  3   3   010300   id    N8   Online      FC  F-Port  21:00:00:24:ff:65:e2:49 
  4   4   010400   id    N8   Online      FC  F-Port  10:00:00:05:1e:fd:98:8c 
  5   5   010500   id    N8   Online      FC  F-Port  10:00:00:05:1e:fd:97:48 
  6   6   010600   id    N8   No_Light    FC 
  7   7   010700   id    N8   No_Light    FC 
  8   8   010800    –    N8   No_Module   FC  (No POD License) Disabled 
  9   9   010900    –    N8   No_Module   FC  (No POD License) Disabled 
 10  10   010a00    –    N8   No_Module   FC  (No POD License) Disabled 
 11  11   010b00    –    N8   No_Module   FC  (No POD License) Disabled 
 12  12   010c00    –    N8   No_Module   FC  (No POD License) Disabled 
 13  13   010d00    –    N8   No_Module   FC  (No POD License) Disabled 
 14  14   010e00    –    N8   No_Module   FC  (No POD License) Disabled 
 15  15   010f00    –    N8   No_Module   FC  (No POD License) Disabled 
 16  16   011000    –    N8   No_Module   FC  (No POD License) Disabled 
 17  17   011100    –    N8   No_Module   FC  (No POD License) Disabled 
 18  18   011200    –    N8   No_Module   FC  (No POD License) Disabled 
 19  19   011300    –    N8   No_Module   FC  (No POD License) Disabled 
 20  20   011400    –    N8   No_Module   FC  (No POD License) Disabled 
 21  21   011500    –    N8   No_Module   FC  (No POD License) Disabled 
 22  22   011600    –    N8   No_Module   FC  (No POD License) Disabled 
 23  23   011700    –    N8   No_Module   FC  (No POD License) Disabled
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
zonecreate "esx2_HBA1_Unity_spa01","esx1_HBA1;Unity_spa01"
zonecreate "esx2_HBA2_Unity_spa01","esx1_HBA2;Unity_spa01"
zonecreate "esx2_HBA1_Unity_spb01","esx1_HBA1;Unity_spb01"
zonecreate "esx2_HBA2_Unity_spb01","esx1_HBA2;Unity_spb01"
```

### 6.保存zone

```
cfgcreate "cfg","esx1_HBA1_Unity_spa01"  //创建配置文件cfg，并添加"esx1_HBA1_Unity_spa01"zone
cfgadd "cfg","esx1_HBA2_Unity_spa01"     //添加"esx1_HBA2_Unity_spa01"zone到cfg配置文件
cfgadd "cfg","esx1_HBA1_Unity_spb01"
cfgadd "cfg","esx1_HBA2_Unity_spb01"
cfgadd "cfg","esx2_HBA1_Unity_spa01"
cfgadd "cfg","esx2_HBA2_Unity_spa01"
cfgadd "cfg","esx2_HBA1_Unity_spb01"
cfgadd "cfg","esx2_HBA2_Unity_spb01"
```

### 7.启用配置文件

```
cfgenable "cfg"   输入y
```

### 8.保存配置

```
cfgsave   输入y
```

## 常用命令

### 1.更改管理地址

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

```
sfpshow 1 -f    //查看1口的光功率
```
