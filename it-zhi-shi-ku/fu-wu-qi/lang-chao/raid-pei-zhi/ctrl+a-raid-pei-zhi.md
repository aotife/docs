---
description: 浪潮服务器Raid配置说明
---

# Ctrl+A  RAID配置

#### 特别说明！

PM8060 RAID卡与其它Ctrl-A系列RAID卡（例如6805/5805等）在JBOD和热备盘的配置方法上不同，区别如下：

1）PM8060 RAID卡配置界面中没有Create JBOD选项，而是通过uninitialize硬盘实现JBOD。

2）PM8060 RAID卡配置界面中没有Global Hotspare选项，需要在BIOS下或通过RAID卡管理工具配置热备。

具体请参考本手册详细内容。\


### 一、RAID卡阵列配置

### **1.1 查看Raid状态**

服务器开机自检到浪潮logo画面后，下一步就会进入Raid卡自检过程，此时显示器上会出现Ctrl -A提示，如下图：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/03/20211023100644.jpg)

Optimal表示raid状态正常，Degraded表示有一块硬盘掉线，阵列降级，Offline表示有两块或以上硬盘掉线，阵列不可用 按下Ctrl -A组合键后，自检完成就会进入Raid卡配置界面，如下图：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/03/20211023100914.jpg)

选择Array Configuration Utility进入配置主界面

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/03/20211023101020.jpg)

选择”Manage Arrays“，此时右侧会出现当前配置的raid信息。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/03/20211023101141.png)

上下键选择要查看的raid VD，回车选择。此时红框内显示阵列状态。OPTIMAL是正常状态。Array Size后显示的是阵列大小。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/03/20211023101313.png)

### **1.2 删除Raid阵列**

首先参考”查看raid“章节，进入raid配置界面，在Manage Array上按回车

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/03/20211026230053.jpg)

按delete则弹出删除界面，选择delete后阵列将被删除。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/20211026230142.jpg)

再次查看已经没有raid信息

![](https://typore-img.oss-cn-beijing.aliyuncs.com/img/imge/2021/20211026230214.jpg)

### **1.3 大存储下Raid配置建议**

当存储比较大且安装Window2008R2时，建议先配置一个较小的虚拟盘（VD），进行操作系统的安装。之后安装结束后，再配置另一个较大的虚拟盘（VD）。

比如创建Raid5时（具体步骤请参考后续各种raid配置章节），在创建VD时进行的大小配置，此时显示的是默认大小（最大可使用空间）。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701100256.png)

在此处输入Raid阵列大小，默认为最大空间，这里可以手工修改，建议改成100G-200G即可（只要小于2TB都可以，只用来当做C盘安装系统）。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701100353.png)

之后，在安装完操作系统后，可以将后续很大的空间创建VD，操作系统会自动识别。这样就避免了安装Windows 2008时C盘所在磁盘不能超过2TB问题。

### **1.4 Raid0的配置**

首先参考”查看raid“章节，进入raid配置界面。选择Create Array进入raid配置界面。

注意：在raid开始配置时，如果硬盘不能选择，请先对硬盘做初始化操作，可参考后续硬盘初始化章节。 **特别提醒：不要初始化有数据的硬盘！！！**

1）选择硬盘，这里以四块硬盘为例，按空格键选择

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701100554.jpeg)

2）选择raid0（注意，如果您需要单盘配置raid0，则这里选择volume）

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701101652.jpeg)

3）输入Array Label，比如volume1

4）输入Array Size（卷大小），默认容量为最大容量

5）选择条带大小，默认为256KB

6）选择Read Caching（读策略），默认为enabled：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701101716.jpeg)

7）选择Write Caching（写策略），默认为Enable always

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701101744.jpeg)

选择Enable always后，会有确认提示，按Y键

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701101829.jpeg)

再次确认，按Y键

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701101901.jpeg)

8）选择Raid创建方式，建议选择Quick init（快速初始化）

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701101920.jpeg)

9）最后选择【Done】回车，出现完成提示时按任意键退出。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701101942.jpeg)

完成配置后可以在Manage Array中查看阵列状态，其中Optimal为正常，Degraded为阵列降级，代表有硬盘掉线，Offline为阵列掉线。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102000.jpeg)

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102021.jpeg)

### **1.5 Raid1的配置**

首先参考”查看raid“章节，进入raid配置界面。选择Create Array进入raid配置界面。

注意：在raid开始配置时，如果硬盘不能选择，请先对硬盘做初始化操作，可参考后续硬盘初始化章节。 **特别提醒：不要初始化有数据的硬盘！！！**

1\) 选择2块硬盘，按空格键选择

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102117.jpeg)

2\) 选择Raid级别

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102134.jpeg)

3）输入Array Label（卷标），如volume1

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102153.jpeg)

4）输入Array Size（卷大小），默认容量为最大容量

5）Array Size（条带大小）默认为N/A，不可选

6）选择Read Caching（读策略），默认为enabled：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102156.jpeg)

7）选择Write Caching（写策略），默认为Enable always

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102405.jpeg)

选择Enable always后，会有确认提示，按Y键

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102204.jpeg)

再次确认，按Y键

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102207.jpeg)

8\) 选择创建raid方式，建议选择Quick Init（快速初始化）

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102212.jpeg)

9）最后选择【Done】回车，出现完成提示按任意键退出，然后在Manage Array中查看raid状态是否配置正常。其中Optimal为正常，Degraded为阵列降级，代表有硬盘掉线，Offline为阵列掉线。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102217.jpeg)

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701103902.jpeg)

### **1.6 Raid5的配置**

首先参考”查看raid”章节，进入raid配置界面。选择Create Array进入raid配置界面。

注意：在raid开始配置时，如果硬盘不能选择，请先对硬盘做初始化操作，可参考后续硬盘初始化章节。 **特别提醒：不要初始化有数据的硬盘！！！**

1）最少选择3块硬盘，这里以3块硬盘为例，按空格键选择

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102639.jpeg)

2）选择Raid级别：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102656.jpeg)

3）输入Array Label（卷标），如volume5

4）输入Array Size（卷大小），默认容量为最大容量

5）Array Size（条带大小）默认为N/A，不可选

6）选择Read Caching（读策略），默认为enabled

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102718.jpeg)

7）选择Write Caching（写策略），默认为Enable always

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102754.jpeg)

选择Enable always后，会有确认提示，按Y

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102425.jpeg)

再次确认，按Y键

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102444.jpeg)

8\) 选择创建raid方式，建议选择Quick Init（快速初始化

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102501.jpeg)

9）最后选择【Done】回车，出现完成提示按任意键退出，然后在Manage Array中查看raid状态是否配置正常。其中Optimal为正常，Degraded为阵列降级，代表有硬盘掉线，Offline为阵列掉

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102518.jpeg)

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701102536.jpeg)

### **1.7 Raid6的配置**

首先参考”查看raid“章节，进入raid配置界面。选择Create Array进入raid配置界面。

注意：在raid开始配置时，如果硬盘不能选择，请先对硬盘做初始化操作，可参考后续硬盘初始化章节。 **特别提醒：不要初始化有数据的硬盘！！！**

1）最少选择4块硬盘，按空格键选择

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701103056.jpeg)

2）选择Raid级别：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701103123.jpeg)

3）输入Array Label（卷标），如volume5

4）输入Array Size（卷大小），默认容量为最大容量

5）Array Size（条带大小）默认为N/A，不可选

6）选择Read Caching（读策略），默认为enabled：

7）选择Write Caching（写策略），默认为Enable always，保持默认即可，会有确认提示，按Y键

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701103145.jpeg)

再次确认，按Y键

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701103203.jpeg)

8）选择创建raid方式，建议选择Quick Init（快速初始化）

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701103222.jpeg)

9）最后选择【Done】回车，出现完成提示按任意键退出，然后在Manage Array中查看raid状态是否配置正常。其中Optimal为正常，Degraded为阵列降级，代表有硬盘掉线，Offline为阵列掉线。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701103244.jpeg)

### **1.8 Raid10的配置**

首先参考”查看raid“章节，进入raid配置界面。选择Create Array进入raid配置界面。

注意：在raid开始配置时，如果硬盘不能选择，请先对硬盘做初始化操作，可参考后续硬盘初始化章节。 **特别提醒：不要初始化有数据的硬盘！！！**

1）最少选择4块硬盘，必须是偶数，按空格键选择。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104034.jpeg)

2）选择Raid级别：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104058.jpeg)

3）后续步骤与创建raid5和raid6类相同，不再赘述。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104114.jpeg)

最后，在Manage Array中查看raid状态是否配置正常。其中Optimal为正常，Degraded为阵列降级，代表有硬盘掉线，Offline为阵列掉线。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104133.jpeg)

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104148.jpeg)

### **1.9 JBOD的配置**

#### **1.9.1 6805 RAID卡**

注意：已经配置RAID的磁盘不能再做JBOD，如磁盘已配置RAID，请先删除（注意备份数据！）。

6805 RAID卡配置界面下有Create JBOD选项，回车进入JBOD配置界面。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104217.jpeg)

进入如下界面，上下键选中需要配置JBOD的磁盘，按Insert键进行选择（如果选错可以按Del键进行反选），选完按回车键确认即可。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104235.jpeg)

按Y键确认

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104254.jpeg)

返回RAID配置界面，选择“Manage JBOD”可以看到所有JBOD磁盘.

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104313.jpeg)

在这里按Del可以删除JBOD，也可以按Ctrl+V将JBOD转换为Volume（单盘raid0），转换后的Volume入下图：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104333.jpeg)

#### **1.9.2 PM8060 RAID卡**

注意：已经配置RAID的磁盘不能再做JBOD，如磁盘已配置RAID，请先删除（注意备份数据！）。

PM8060 RAID卡配置界面下没有Create JBOD选项，而是通过uninitialize硬盘实现JBOD。进入RAID卡配置界面，选择“Uninitialize Drivers”进入JBOD配置界面。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104409.png)

进入如下界面，上下键选中需要配置JBOD的磁盘，按Insert键进行选择（如果选错可以按Del键进行反选），选完按回车键确认即可。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104431.png)

### **1.10 热备盘（Hotspare）配置**

#### **1.10.1 6805 RAID卡**

热备盘的作用是如果阵列中有硬盘发生故障，热备盘可以立即顶替，及时将阵列恢复为正常状态。

6805 RAID卡配置界面下有Global Hotspare选项，回车进入热备盘配置界面。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104458.jpeg)

有提示信息，按任意键继续。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104516.jpeg)

左侧列表显示当前所有硬盘，可配置热备的硬盘为白色高亮显示，已配置RAID的磁盘盘则是灰色不可选。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104535.jpeg)

空格选择硬盘

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104552.jpeg)

回车后会有提示是否保存，按Y键确认。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104608.jpeg)

#### **1.10.2 PM8060 RAID卡**

热备盘的作用是如果阵列中有硬盘发生故障，热备盘可以立即顶替，及时将阵列恢复为正常状态。

PM8060 RAID卡配置界面没有”Global Hotspares”选项，可以在操作系统下使用 [**arcconf**](http://www.4008600011.com/wp-content/uploads/2018/01/arcconf.zip) 工具配置，有些机型也支持在BIOS中配置（**NF84XX/TS8xx的服务器不支持在BIOS中配置**）。

不同机型的BIOS界面可能不同，有两种界面配置方法。

**BIOS界面一（此配置方法目前对于双路平台机器可以正常使用，NF84XX/TS8xx的服务器由于BIOS不同，需要系统下使用工具配置热备。）**

进入BIOS — Advanced — CSM Configuration菜单，将Storage选项设置为UEFI，然后F10保存退出。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104630.jpeg)

重启后再次进入BIOS — Advanced — PMC maxView Storage Manager菜单：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104647.jpeg)

BIOS扫描到RAID卡后会出现如下菜单，进入Logical Device Configuration — Global Hotspare菜单：

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104704.jpeg)

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104720.jpeg)

将未配置的硬盘Enable，然后在【ADD】上回车，再Submit提交即可，提示成功后按任意键返回。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104738.jpeg)

最后，请将BIOS选项还原。进入BIOS — Advanced — CSM Configuration菜单，将Storage选项设置为Legacy，F10保存退出。

**BIOS界面二（此配置方法不适用于NF84XX/TS8xx的服务器，NF84XX/TS8xx的服务器由于BIOS不同，需要系统下使用工具配置热备。）**

重启进入BIOS中，在Boot下将Boot Type改为”UEFI Boot Type“

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104757.png)

按F10保存退出

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104818.png)

服务器重启后，在如下界面按DEL键进入Setup界面

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104822.png)

此时的Setup界面和之前的并不一样，多了”Device Management“选项，选择此选项。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104826.png)

选择”PMC maxView Storage Management”进行存储的配置

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104833.png)

回车选择”Scan For Controllers”

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104837.png)

回车选择raid卡控制器

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104841.png)

选择”Logical Device Configuration”

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104845.png)

选择”Global Hotspares”进行热备盘的配置

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104852.png)

回车确认告警

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104858.png)

选择”Add Spares”

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104902.png)

空格键选择热备盘，选择后，磁盘的状态变为”X”。移动到”ADD”项后按回车

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104906.png)

回车确认告警

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104910.png)

热备盘添加成功

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104915.png)

重启后再进入BIOS配置中，将Boot列表下的”Boot Type”改回至”Legacy Boot Type”

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701104919.png)

**操作系统下使用arcconf工具配置热备盘。**

Windows系统下：

1.先解压，然后运行里输入cmd，打开cmd命令后，cd到arcconf.exe所在目录下。

2.查看硬盘状态，确认需要配置热备的物理磁盘的channel号和device号

**arcconf.exe  getconfig 1 pd**

其中的数字1表示raid卡1.

查看返回信息中硬盘状态是ready，状态ready是没有使用的磁盘，把需要配置成热备的硬盘的 channel号和device号记录下来。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105326.jpeg)

3.配置热备： arcconf.exe  setstate 1 device _\<Channel#>   \<Device#>_  hsp

其&#x4E2D;_\<Channel#>_ &#x548C;_\<Device#>_&#x4EE5;实际的channel号和device编号为准，例如硬盘的channel号是0，device号是2，实际命令为：arcconf.exe setstate 1 device _0  2_  hsp

可以通过**arcconf.exe  getconfig 1 pd 查看硬盘状态是否配置成功。**

2\. Linux系统下的命令是

**查看硬盘：./arcconf  getconfig 1 pd**

**配置热备： ./arcconf  setstate 1 device &#x20;**_**\<Channel#>   \<Device#>**_**&#x20; hsp**

### **1.11 控制器属性配置**

在Raid配置界面，选择”Controller Settings”进行控制器属性的配置

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105335.png)

选择”Controller Configuration”

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105339.png)

此时，即显示raid卡的属性配置界面，可以进行查看和修改

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105343.png)

常用配置项\


Alarm Control：阵列卡告警设置\


Enable：故障时正常蜂鸣\


Disable：故障时不蜂鸣\


Silence：本次不蜂鸣

## 二、常见问题的故障恢复

重要提示：服务器通过Raid技术可以有效增强数据的安全性，但是不代表做了Raid就永远不会出问题，所以数据还是要经常备份的！

一般我们最常遇到的问题就是有硬盘亮红灯了，有些时候还会有报警声。但是请不要担心，硬盘亮红灯不代表硬盘一定有故障，那么哪些情况会导致硬盘亮红灯呢？

1、人为拔插过硬盘

2、硬盘没有插到位，接触不良

3、意外停电，影响了阵列信息

4、硬盘发生逻辑上的I/O错误

5、硬盘本身故障

如果您是新机器，硬盘亮红灯大多是因为物流等原因，可能某块硬盘没有插到位，接触不良；如果已经使用了一段时间，大多是因为硬盘发生了逻辑上的I/O错误，因为做了Raid以后，需要多块硬盘协同工作，不仅要把文件打碎，还要一起计算校验值，如果在某一块硬盘上计算错误，可能会导致硬盘被踢出阵列，同时亮红灯报警。如果服务器灰尘较多，容易积蓄静电，也会增加硬盘出错的概率。

### **2.1 单盘掉线及处理步骤**

以raid5为例，如果有一块硬盘掉线，raid阵列将变为degraded，数据仍可正常读写

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105405.jpeg)

更换硬盘后会自动同步，显示rebuild进度

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105409.jpeg)

Rebuild完成后，阵列会回到Optimal状态

### **2.2 多盘掉线及处理步骤**

以raid5举例，如果阵列中有两块或以上的盘掉线，则raid状态变为Offline，此时数据不可用，此时需要找数据恢复公司进行数据的恢复。

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105414.jpeg)

### **2.3 初始化硬盘**

如果新更换的硬盘有raid信息或raid信息不正确而导致无法使用时，需要将该硬盘初始化

首先要明白初始化硬盘的意义，当想将硬盘进行raid配置时，需要进行硬盘的初始化，如果不初始化，硬盘则不能进行raid配置。

首先参考”查看raid“章节，进入raid配置界面。选择Initialize Drives

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105419.jpeg)

选择要初始化的硬盘

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105423.jpeg)

按空格键选择硬盘

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105427.jpeg)

弹出警告，需要按Y确认

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105431.jpeg)

再次确认

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105435.jpeg)

初始化成功

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105441.jpeg)

### **2.4 校验硬盘**

当需要检测硬盘时，可以通过校验硬盘来查看硬盘是否有坏道

在主界面中选择Disk Utilities查看物理硬盘列表

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105447.jpeg)

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105453.jpeg)

选择需要校验的硬盘，回车

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105457.jpeg)

第一项为低级格式化硬盘（强烈建议如非必要勿选，若误操作选中则不要停止，让其格式化完成，否则硬盘会损坏），第二项为校验硬盘，第三项为定位硬盘

选择Verify Disk Media

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105502.jpeg)

确认后进入校验界面

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105506.jpeg)

若硬盘正常则一直校验，直至100%，提示completed，若有坏道，则弹出报错提示，需要更换硬盘

![](https://typore-img.oss-cn-beijing.aliyuncs.com/Typecho/img/2022/20220701105510.jpeg)

#### 引用：

{% embed url="http://www.4008600011.com/archives/9346" %}

