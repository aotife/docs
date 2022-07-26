# 启用MWAIT等

**故障现象：**

服务器加入集群或启用EVC模式时，报错：

主机的CPU硬件应支持集群当前的增强型vMotion兼容性模式，但主机现在缺少某些必要的CPU功能。请检查主机的BIOS配置，确保未禁用必要的功能（例如Intel的XD、VT、AES或PCLMULQDQ，或者AMD的NX）

**解决方案**

1、对于双路M4服务器，Intel XD默认已开启，HT、VT和AES选项位置分别如下，请确保处于Enable状态。

![img](https://pic.chjina.com/2022/11/10/VMWT-29.png)

2、vMotion功能还依赖于Mwait，然而2017年4月8号之后生产的通用双路M4机器已经默认关闭了此功能，请开启。

老版BIOS：

`Chipset -> Advance Power Management Configuration -> CPU C State Control -> Monitor/Mwait Support`

新版BIOS：

`Chipset -> Power/Performance Configuration -> Monitor/Mwait Support`

另外，打开Mwait后相当于服务器启用了休眠功能，请登录到Vmware系统下，将电源选项设置为高性能模式。

**引用：**

{% embed url="http://www.4008600011.com/archives/419#10_VMwareEVCCPU" %}
