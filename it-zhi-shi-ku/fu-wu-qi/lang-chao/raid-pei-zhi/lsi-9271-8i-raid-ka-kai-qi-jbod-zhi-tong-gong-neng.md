---
description: 服务器型号：NF5280NF
---

# LSI 9271 8i Raid卡开启jbod(直通)功能

#### 故障现象

LSI 9271-8i raid卡默认无法将硬盘作为jbod，没有jbod选项

#### 涉及范围

LSI9271-8i raid卡

#### 解决方法

需使用命令开启jbod功能，具体命令如下:MegaCli -AdpSetProp -EnableJBOD -1 -a0

1. 自检到raid卡，按【Ctrl Y】进入命令行：

![img](https://pic.chjina.com/2022/11/10/010219\_1037\_3.png)

1.  敲入`-AdpSetProp -EnableJBOD -1 -a0`

    ![img](https://pic.chjina.com/2022/11/10/010219\_1037\_4.png)
2. 显示`Set JBOD to Enable success` 就是启用成功。

####
