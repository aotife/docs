# 华为2288H v5服务器开启MWAIT功能

近几天装了几台华为的2288Hv5的服务器做虚拟化，使用的VMware7.0.3 ，提示**缺少功能“MWAIT”，但此功能必须存在，无法启动虚拟机，模块“FeatureCompatLate”打开电源失败**

![img](https://pic.chjina.com/2022/08/22/1647224264922.png)

看来下是要开启MWAIT功能，

本文介绍下华为2288H v5服务器开启MWAIT功能，

### 进入BIOS

* 开机引导按`Delete`进入BIOS，默认密码`Admin@9000`

#### 1.选择高级——槽位配置

![image-20220914154430302](https://pic.chjina.com/2022/09/14/20220914154524.png)

#### 2.选择CPU配置

![img](https://pic.chjina.com/2022/08/22/1647224963932.png)

#### 3.选择MONITOR/MWAIT

![img](https://pic.chjina.com/2022/08/22/1647224966131.png)

#### 4.开启MONITOR/MWAIT指令

![img](https://pic.chjina.com/2022/08/22/1647224967628.png)

引用参考：

{% embed url="https://blog.csdn.net/xiezuoyong/article/details/123686809" %}

{% embed url="http://www.hnkddz.com/article-detail/BAlplZ6b" %}

{% embed url="http://support.xfusion.com/support/#/zh/docOnline/EDOC1000163371?path=zh-cn_topic_0000001136230833" %}
