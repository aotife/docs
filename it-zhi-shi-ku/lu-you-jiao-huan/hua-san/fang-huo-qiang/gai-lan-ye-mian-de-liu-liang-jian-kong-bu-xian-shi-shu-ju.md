# 概览页面的流量监控不显示数据

### 华三防火墙概览页面的流量监控不显示数据。

如图所示：

![微信图片\_20220817160353](https://pic.chjina.com/2022/08/17/20220.png)

### 解决办法：

**如下命令都开启，然后等待一段时间，再登陆到设备上看看**

**`Info-center enable` 开启信息中心功能 （非必须）**

**`webui log enable` 开启Web操作日志功能（非必须）**

**`session statistics enable` 用来开启会话统计功能（建议）**

**`session top-statistics enable` 开启会话数目的Top排名统计功能（建议）**

**`application global statistics enable` 开启全局应用统计功能（建议）**

**`inspect activate` 激活策略和规则（非必须）**

### 生效 ：

![image-20220817162129388](https://pic.chjina.com/2022/08/17/image-20220817162129388.png)
