# 华为2288H v5服务器开启MWAIT功能

近几天装了几台华为的2288Hv5的服务器做虚拟化，使用的VMware7.0.3 ，提示**缺少功能“MWAIT”，但此功能必须存在，无法启动虚拟机，模块“FeatureCompatLate”打开电源失败**

![img](https://pic.chjina.com/2022/08/22/1647224264922.png)

看来下是要开启MWAIT功能，

本文介绍下华为2288H v5服务器开启MWAIT功能，

### 进入BIOS

* 开机引导按`Delete`进入BIOS，默认密码`Admin@9000`

![img](https://pic.chjina.com/2022/08/22/1647224963932.png)

![img](https://pic.chjina.com/2022/08/22/1647224966131.png)

![img](https://pic.chjina.com/2022/08/22/1647224967628.png)

引用参考：

* [vCLS报错处理（缺少功能“MWAIT”，没有与虚拟机兼容的主机）\_白昼ron的博客-CSDN博客\_没有与虚拟机兼容的主机](https://blog.csdn.net/xiezuoyong/article/details/123686809)
* [MWAIT (hnkddz.com)](http://www.hnkddz.com/article-detail/BAlplZ6b)
* [超聚变数字技术有限公司技术支持 (xfusion.com)](http://support.xfusion.com/support/#/zh/docOnline/EDOC1000163371?path=zh-cn\_topic\_0000001136230833)
