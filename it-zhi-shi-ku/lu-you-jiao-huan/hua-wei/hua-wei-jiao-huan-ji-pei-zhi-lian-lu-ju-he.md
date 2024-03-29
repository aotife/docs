# 华为交换机配置链路聚合

## 配置静态链路聚合示例（交换机之间直连）

### 手工模式链路聚合简介

以太网链路聚合是指将多条以太网物理链路捆绑在一起成为一条逻辑链路，从而实现增加链路带宽的目的。链路聚合分为手工模式和LACP模式。

手工模式下，Eth-Trunk的建立、成员接口的加入由手工配置，没有链路聚合控制协议LACP的参与。当需要在两个直连设备间提供一个较大的链路带宽而设备又不支持LACP协议时，可以使用手工模式。手工模式可以实现增加带宽、提高可靠性、负载分担的目的。

手工模式下，所有的活动链路都参与数据转发并分担流量。

### 配置注意事项

* 一个Eth-Trunk接口中的成员接口必须是以太网类型和速率相同的接口。
* Eth-Trunk链路两端相连的物理接口的数量、速率、双工方式、流控配置必须一致。
* 如果本端设备接口加入了Eth-Trunk，与该接口直连的对端接口也必须加入Eth-Trunk，两端才能正常通信。
* 两台设备对接时需要保证两端设备上链路聚合的模式一致。
* 本举例适用于S系列交换机所有产品的所有版本。

### 组网需求

如[图3-75](https://support.huawei.com/enterprise/zh/doc/EDOC1000069491/5d18b4c0#fig\_dc\_cfg\_eth-trunk\_003401)所示，SwitchA和SwitchB通过以太链路分别都连接VLAN10和VLAN20的网络，且SwitchA和SwitchB之间有较大的数据流量。

用户希望SwitchA和SwitchB之间能够提供较大的链路带宽来使相同VLAN间互相通信。同时用户也希望能够提供一定的冗余度，保证数据传输和链路的可靠性。

**图3-75** 配置手工模式链路聚合组网图

&#x20;

<figure><img src="https://pic.chjina.com/2023/07/24/download" alt=""><figcaption></figcaption></figure>

### 配置思路

采用如下的思路配置手工模式链路聚合：

1. 创建Eth-Trunk接口并加入成员接口，实现增加链路带宽。
2. 创建VLAN并将接口加入VLAN。
3. 配置负载分担方式，实现流量在Eth-Trunk各成员接口间的负载分担，增加可靠性。

### 操作步骤

1.  在SwitchA和SwitchB上创建Eth-Trunk接口并加入成员接口

    ```
    <HUAWEI> system-view
    [HUAWEI] sysname SwitchA
    [SwitchA] interface eth-trunk 1   //创建ID为1的Eth-Trunk接口
    [SwitchA-Eth-Trunk1] trunkport gigabitethernet 1/0/1 to 1/0/3   //在Eth-Trunk1接口中加入GE1/0/1到GE1/0/3三个成员接口
    [SwitchA-Eth-Trunk1] quit
    ```

    ```
    <HUAWEI> system-view
    [HUAWEI] sysname SwitchB
    [SwitchB] interface eth-trunk 1   //创建ID为1的Eth-Trunk接口
    [SwitchB-Eth-Trunk1] trunkport gigabitethernet 1/0/1 to 1/0/3   //在Eth-Trunk1接口中加入GE1/0/1到GE1/0/3三个成员接口
    [SwitchB-Eth-Trunk1] quit
    ```
2.  创建VLAN并将接口加入VLAN

    \# 创建VLAN10和VLAN20并分别加入接口。SwitchB的配置与SwitchA类似，不再赘述。

    ```
    [SwitchA] vlan batch 10 20
    [SwitchA] interface gigabitethernet 1/0/4
    [SwitchA-GigabitEthernet1/0/4] port link-type trunk   //设置接口链路类型为trunk，接口缺省链路类型不是trunk口
    [SwitchA-GigabitEthernet1/0/4] port trunk allow-pass vlan 10
    [SwitchA-GigabitEthernet1/0/4] quit
    [SwitchA] interface gigabitethernet 1/0/5
    [SwitchA-GigabitEthernet1/0/5] port link-type trunk   //设置接口链路类型为trunk，接口缺省链路类型不是trunk口
    [SwitchA-GigabitEthernet1/0/5] port trunk allow-pass vlan 20
    [SwitchA-GigabitEthernet1/0/5] quit
    ```

    \# 配置Eth-Trunk1接口允许VLAN10和VLAN20通过。SwitchB的配置与SwitchA类似，不再赘述。

    ```
    [SwitchA] interface eth-trunk 1
    [SwitchA-Eth-Trunk1] port link-type trunk   //设置接口链路类型为trunk，接口缺省链路类型不是trunk口
    [SwitchA-Eth-Trunk1] port trunk allow-pass vlan 10 20
    [SwitchA-Eth-Trunk1] quit
    ```
3.  配置Eth-Trunk1的负载分担方式。SwitchB的配置与SwitchA类似，不再赘述。

    ```
    [SwitchA] interface eth-trunk 1
    [SwitchA-Eth-Trunk1] load-balance src-dst-mac   //配置Eth-Trunk1基于源MAC地址与目的MAC地址进行负载分担
    [SwitchA-Eth-Trunk1] quit
    ```
4.  验证配置结果

    在任意视图下执行**display eth-trunk 1**命令，检查Eth-Trunk是否创建成功，及成员接口是否正确加入。

    ```
    [SwitchA] display eth-trunk 1
    Eth-Trunk1's state information is: 
    WorkingMode: NORMAL           Hash arithmetic: According to SA-XOR-DA
    Least Active-linknumber: 1     Max Bandwidth-affected-linknumber: 8
    Operate status: up             Number Of Up Port In Trunk: 3 
    --------------------------------------------------------------------------------
    PortName                           Status       Weight
    GigabitEthernet1/0/1               Up           1
    GigabitEthernet1/0/2               Up           1
    GigabitEthernet1/0/3               Up           1
    ```

    从以上信息看出Eth-Trunk 1中包含3个成员接口GigabitEthernet1/0/1、GigabitEthernet1/0/2和GigabitEthernet1/0/3，成员接口的状态都为Up。Eth-Trunk 1的“Operate status”为**up**。



## 配置LACP模式的链路聚合示例（交换机之间直连）

### LACP模式链路聚合简介

以太网链路聚合是指将多条以太网物理链路捆绑在一起成为一条逻辑链路，从而实现增加链路带宽的目的。链路聚合分为手工模式和LACP模式。

LACP模式需要有链路聚合控制协议LACP的参与。当需要在两个直连设备间提供一个较大的链路带宽而设备支持LACP协议时，建议使用LACP模式。LACP模式不仅可以实现增加带宽、提高可靠性、负载分担的目的，而且可以提高Eth-Trunk的容错性、提供备份功能。

LACP模式下，部分链路是活动链路，所有活动链路均参与数据转发。如果某条活动链路故障，链路聚合组自动在非活动链路中选择一条链路作为活动链路，参与数据转发的链路数目不变。

### 配置注意事项

* 一个Eth-Trunk接口中的成员接口必须是以太网类型和速率相同的接口。
* Eth-Trunk链路两端相连的物理接口的数量、速率、双工方式、流控配置必须一致。
* 如果本端设备接口加入了Eth-Trunk，与该接口直连的对端接口也必须加入Eth-Trunk，两端才能正常通信。
* 两台设备对接时需要保证两端设备上链路聚合的模式一致。
* 本举例适用于S系列交换机所有产品的所有版本。

### 背景信息

如[图3-76](https://support.huawei.com/enterprise/zh/doc/EDOC1000069491/5d18b4c0#fig\_dc\_cfg\_eth-trunk\_003501)所示，SwitchA和SwitchB通过以太链路分别都连接VLAN10和VLAN20的网络，且SwitchA和SwitchB之间有较大的数据流量。用户希望SwitchA和SwitchB之间能够提供较大的链路带宽来使相同VLAN间互相通信。在两台Switch设备上配置LACP模式链路聚合组，提高两设备之间的带宽与可靠性，具体要求如下：

* 两条活动链路具有负载分担的能力。
* 两设备间的链路具有1条冗余备份链路，当活动链路出现故障时，备份链路替代故障链路，保持数据传输的可靠性。
* 同VLAN间可以相互通信。

**图3-76** 配置LACP模式链路聚合组网图

&#x20;

<figure><img src="https://pic.chjina.com/2023/07/24/download" alt=""><figcaption></figcaption></figure>

### 配置思路

采用如下的思路配置LACP模式链路聚合：

1. 创建Eth-Trunk，配置Eth-Trunk为LACP模式，实现链路聚合功能。
2. 将成员接口加入Eth-Trunk。
3. 配置系统优先级，确定主动端，按照主动端设备的接口选择活动接口。
4. 配置活动接口上限阈值，实现保证带宽的情况下提高网络的可靠性。
5. 配置接口优先级，确定活动链路接口，优先级高的接口将被选作活动接口。
6. 创建VLAN并将接口加入VLAN。

### 操作步骤

1.  在SwitchA上创建Eth-Trunk1并配置为LACP模式。SwitchB的配置与SwitchA类似，不再赘述

    ```
    <HUAWEI> system-view
    [HUAWEI] sysname SwitchA
    [SwitchA] interface eth-trunk 1   //创建ID为1的Eth-Trunk接口
    [SwitchA-Eth-Trunk1] mode lacp   //配置链路聚合模式为LACP模式
    [SwitchA-Eth-Trunk1] quit
    ```
2.  配置SwitchA上的成员接口加入Eth-Trunk1。SwitchB的配置与SwitchA类似，不再赘述

    ```
    [SwitchA] interface gigabitethernet 1/0/1
    [SwitchA-GigabitEthernet1/0/1] eth-trunk 1   //将GE1/0/1接口加入Eth-Trunk1中
    [SwitchA-GigabitEthernet1/0/1] quit
    [SwitchA] interface gigabitethernet 1/0/2
    [SwitchA-GigabitEthernet1/0/2] eth-trunk 1   //将GE1/0/2接口加入Eth-Trunk1中
    [SwitchA-GigabitEthernet1/0/2] quit
    [SwitchA] interface gigabitethernet 1/0/3
    [SwitchA-GigabitEthernet1/0/3] eth-trunk 1   //将GE1/0/3接口加入Eth-Trunk1中
    [SwitchA-GigabitEthernet1/0/3] quit
    ```
3.  在SwitchA上配置系统优先级为100，使其成为LACP主动端

    ```
    [SwitchA] lacp priority 100   //系统LACP优先级缺省为32768，修改SwitchA的优先级大于SwitchB的优先级，作为主动端
    ```
4.  在SwitchA上配置活动接口上限阈值为2

    ```
    [SwitchA] interface eth-trunk 1
    [SwitchA-Eth-Trunk1] max active-linknumber 2   //链路聚合组活动接口数的上限阈值缺省是8，修改活动接口数的上限阈值为2
    [SwitchA-Eth-Trunk1] quit
    ```
5.  在SwitchA上配置接口优先级确定活动链路

    ```
    [SwitchA] interface gigabitethernet 1/0/1
    [SwitchA-GigabitEthernet1/0/1] lacp priority 100   //接口LACP优先级缺省为32768，修改GE1/0/1接口的LACP优先级为100，作为活动接口
    [SwitchA-GigabitEthernet1/0/1] quit
    [SwitchA] interface gigabitethernet 1/0/2
    [SwitchA-GigabitEthernet1/0/2] lacp priority 100   //接口LACP优先级缺省为32768，修改GE1/0/2接口的LACP优先级为100，作为活动接口
    [SwitchA-GigabitEthernet1/0/2] quit
    ```
6.  创建VLAN并将接口加入VLAN。

    \# 创建VLAN10和VLAN20并分别加入接口。SwitchB的配置与SwitchA类似，不再赘述。

    ```
    [SwitchA] vlan batch 10 20
    [SwitchA] interface gigabitethernet 1/0/4
    [SwitchA-GigabitEthernet1/0/4] port link-type trunk   //设置接口链路类型为trunk，接口缺省链路类型不是trunk口
    [SwitchA-GigabitEthernet1/0/4] port trunk allow-pass vlan 10
    [SwitchA-GigabitEthernet1/0/4] quit
    [SwitchA] interface gigabitethernet 1/0/5
    [SwitchA-GigabitEthernet1/0/5] port link-type trunk   //设置接口链路类型为trunk，接口缺省链路类型不是trunk口
    [SwitchA-GigabitEthernet1/0/5] port trunk allow-pass vlan 20
    [SwitchA-GigabitEthernet1/0/5] quit
    ```

    \# 配置Eth-Trunk1接口允许VLAN10和VLAN20通过。SwitchB的配置与SwitchA类似，不再赘述。

    ```
    [SwitchA] interface eth-trunk 1
    [SwitchA-Eth-Trunk1] port link-type trunk   //设置接口链路类型为trunk，接口缺省链路类型不是trunk口
    [SwitchA-Eth-Trunk1] port trunk allow-pass vlan 10 20
    [SwitchA-Eth-Trunk1] quit
    ```
7.  验证配置结果

    \# 查看各Switch设备的Eth-Trunk信息，查看链路是否协商成功。

    ```
    [SwitchA] display eth-trunk 1
    Eth-Trunk1's state information is:
    Local:                                                                          
    LAG ID: 1                       WorkingMode: LACP                             
    Preempt Delay: Disabled         Hash arithmetic: According to SIP-XOR-DIP       
    System Priority: 100            System ID: 00e0-fca8-0417
    Least Active-linknumber: 1      Max Active-linknumber: 2                        
    Operate status: up              Number Of Up Port In Trunk: 2
    --------------------------------------------------------------------------------
    ActorPortName                    Status     PortType PortPri   PortNo PortKey   PortState  Weight
    GigabitEthernet1/0/1             Selected  1GE       100      6145    2865      11111100     1
    GigabitEthernet1/0/2             Selected  1GE       100      6146    2865      11111100     1
    GigabitEthernet1/0/3             Unselect  1GE       32768    6147    2865      11100000     1

    Partner:
    --------------------------------------------------------------------------------
    ActorPortName                     SysPri    SystemID    PortPri PortNo PortKey   PortState
    GigabitEthernet1/0/1              32768  00e0-fca6-7f85  32768     6145   2609      11111100
    GigabitEthernet1/0/2              32768  00e0-fca6-7f85  32768     6146   2609      11111100
    GigabitEthernet1/0/3              32768  00e0-fca6-7f85  32768     6147   2609      11110000
    ```

    ```
    [SwitchB] display eth-trunk 1
    Eth-Trunk1's state information is:
    Local:
    LAG ID: 1                      WorkingMode: LACP
    Preempt Delay: Disabled        Hash arithmetic: According to SIP-XOR-DIP
    System Priority: 32768         System ID: 00e0-fca6-7f85
    Least Active-linknumber: 1     Max Active-linknumber: 8
    Operate status: up             Number Of Up Port In Trunk: 2
    --------------------------------------------------------------------------------
    ActorPortName                   Status     PortType    PortPri   PortNo  PortKey   PortState  Weight
    GigabitEthernet1/0/1            Selected  1GE        32768      6145    2609      11111100     1
    GigabitEthernet1/0/2            Selected  1GE        32768      6146    2609      11111100     1
    GigabitEthernet1/0/3            Unselect  1GE        32768      6147    2609      11110000     1

    Partner:
    --------------------------------------------------------------------------------
    ActorPortName                     SysPri    SystemID     PortPri  PortNo  PortKey   PortState
    GigabitEthernet1/0/1              100    00e0-fca8-0417  100      6145     2865      11111100
    GigabitEthernet1/0/2              100    00e0-fca8-0417  100      6146     2865      11111100
    GigabitEthernet1/0/3              100    00e0-fca8-0417  32768    6147     2865      11100000
    ```

    通过以上显示信息可以看到，SwitchA的系统优先级为100，高于SwitchB的系统优先级。Eth-Trunk的成员接口中GigabitEthernet1/0/1、GigabitEthernet1/0/2成为活动接口，处于“Selected”状态，接口GigabitEthernet1/0/3处于“Unselect”状态，同时实现M条链路的负载分担和N条链路的冗余备份功能。







## 其他说明：

1.  ### 设置活动端口

    ![](https://pic.chjina.com/2023/07/24/1.png)

```
[SwitchA] interface eth-trunk 1
[SwitchA-Eth-Trunk1] max active-linknumber 2   //链路聚合组活动接口数的上限阈值缺省是8，修改活动接口数的上限阈值为2
[SwitchA-Eth-Trunk1] quit

```

**只需要在SwitchA上即可，不需要两端都配置。**













引用：



{% embed url="https://support.huawei.com/enterprise/zh/doc/EDOC1000069491/5d18b4c0" %}

{% embed url="https://support.huawei.com/enterprise/zh/doc/EDOC1000141427/f36b09a2" %}

