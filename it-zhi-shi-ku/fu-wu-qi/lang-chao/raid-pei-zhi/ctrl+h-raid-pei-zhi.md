---
description: NF5280M4
---

# Ctrl+H RAID配置

## Ctrl+H RAID卡配置步骤

**说明**

本手册适用于LSI芯片Raid卡

包括但不限于Inspur 2008/2108 Raid卡、LSI 9240/9260/9261/ 9271 等Raid卡。

不同型号的Raid卡在某些功能上的支持性不同（例如Inspur2008 Raid卡需要有授权才支持Raid5），具体因您的配置而定。

## 一、Raid配置与管理

### 1．查看raid状态

服务器开机自检到浪潮logo画面后，下一步就会进入Raid卡自检过程，此时显示器上会出现Ctrl -H提示，如下图：

&#x20;

<figure><img src="https://pic.chjina.com/2022/11/10/h1-1.png" alt=""><figcaption></figcaption></figure>

按下Ctrl -H组合键后，自检完成就会进入Raid卡配置界面，如下图。在这里可以看到Raid卡的型号和Firmware固件版本，点击【Start】按钮进入Raid卡主页。

&#x20;

<figure><img src="https://pic.chjina.com/2022/11/10/h02.png" alt=""><figcaption></figcaption></figure>

Raid卡首页叫作WebBIOS，如下图。左侧是功能菜单，右侧可以看到所有物理磁盘，本例安装了4块500G硬盘，后面所有的配置都可以在这里完成！

![img](https://pic.chjina.com/2022/11/10/H03.png)

点击左侧的Logical View进行raid状态的查看。此时右侧显示raid状态为“Optimal”（即正常状态），raid类型为raid5。

![img](https://pic.chjina.com/2022/11/10/H04.png)

### 2．删除单个Raid阵列

当系统中已经创建了多个阵列后，如果只想删除一个阵列，可以依照以下操作进行。

如下图所示，当前有一个阵列，选择左侧的“Virtual Drives”

![img](https://pic.chjina.com/2022/11/10/H05.png)

选择要删除的阵列，以及点选“Properties”，点击Go

![img](https://pic.chjina.com/2022/11/10/H06.png)

此处选择Operations里的“Delete”，点击Go

![img](https://pic.chjina.com/2022/11/10/H07.png)

删除阵列后，数据会丢失，此处需要确认，是否继续删除，选择“Yes”

![img](https://pic.chjina.com/2022/11/10/H08.png)

之后再检查raid配置，会发现当前raid已经删除。

### 3．删除所有Raid阵列

WebBIOS主页点击【Configuration Wizard】，打开配置向导

&#x20;选择【Clear Configuration】，点击【Next】下一步

<figure><img src="https://pic.chjina.com/2022/11/10/H09.png" alt=""><figcaption></figcaption></figure>

&#x20;提示清除，选择【yes】

<figure><img src="https://pic.chjina.com/2022/11/10/H10-2.png" alt=""><figcaption></figcaption></figure>

&#x20;阵列删除成功！所有硬盘显示为蓝色unconfigured Good状态

<figure><img src="https://pic.chjina.com/2022/11/10/H11-1.png" alt=""><figcaption></figcaption></figure>

![img](https://pic.chjina.com/2022/11/10/H12-1.png)

### 4．大存储下Raid配置建议

当存储大于2T且准备安装Window系统时，建议先配置一个较小的虚拟盘（VD），大约150G\~250G，进行操作系统的安装。之后安装结束后，再配置另一个较大的虚拟盘（VD）。

可以参考视频如下：

（Lsi raid 1DG TO 2VD视频：http://pan.baidu.com/s/1bn1Z2IN ）

首先按正常配置把第一个VD创建分区完成到如下截图，可见右侧只有一个VD（注意要选择back噢）

![img](https://pic.chjina.com/2022/11/10/H13-800x529.png)

选择Add To Span

![img](https://pic.chjina.com/2022/11/10/H14-800x567.png)

点击“Next”

![img](https://pic.chjina.com/2022/11/10/H15-800x537.png)

此时选择“Next”

![img](https://pic.chjina.com/2022/11/10/H16-800x569.png)

注意：update size是选择剩余容量，或者可以自行输入容量大小（注意不要大于剩余容量）

![img](https://pic.chjina.com/2022/11/10/H17-800x577.png)

大功告成。

### 5．Raid5的配置

在WebBIOS主页点击【Configuration Wizard】，打开配置向导

&#x20;选择【Add Configuration】，点击【Next】下一步

<figure><img src="https://pic.chjina.com/2022/11/10/H18-5.png" alt=""><figcaption></figcaption></figure>

&#x20;选择【Manual Configuration】，点击【Next】下一步

<figure><img src="https://pic.chjina.com/2022/11/10/H19-5.png" alt=""><figcaption></figcaption></figure>

&#x20;左侧方框内可以看到所有未使用的硬盘。我们选择全部（也可以逐个选择），然后点击下方的【Add to Array】将其加入到右侧方框内。

<figure><img src="https://pic.chjina.com/2022/11/10/H20-5.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Accept DG】，创建磁盘组

<figure><img src="https://pic.chjina.com/2022/11/10/H21-5.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Next】下一步

<figure><img src="https://pic.chjina.com/2022/11/10/H23-5.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Add to SPAN】，将刚才创建好的磁盘组加入到右侧方框内

<figure><img src="https://pic.chjina.com/2022/11/10/H23-5.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Next】下一步

<figure><img src="https://pic.chjina.com/2022/11/10/H24-5.png" alt=""><figcaption></figcaption></figure>

&#x20;阵列参数配置：第一个参数“Raid Level”选择Raid5，其余保持默认

<figure><img src="https://pic.chjina.com/2022/11/10/H25-5.png" alt=""><figcaption></figcaption></figure>

&#x20;最后一个参数“Select Size”输入阵列容量大小，最大值可参考右侧绿字提示（其中R5代表做Raid5的最大容量），完成后点击【Accept】

<figure><img src="https://pic.chjina.com/2022/11/10/H26-5.png" alt=""><figcaption></figcaption></figure>

&#x20;弹出的任何提示均选择【yes】

<figure><img src="https://pic.chjina.com/2022/11/10/H27-5.png" alt=""><figcaption></figcaption></figure>

&#x20;回到配置页面，点击【Next】下一步

<figure><img src="https://pic.chjina.com/2022/11/10/H28-5.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Accept】配置完成！

<figure><img src="https://pic.chjina.com/2022/11/10/H29-5.png" alt=""><figcaption></figcaption></figure>

&#x20;提示保存，选择【yes】

<figure><img src="https://pic.chjina.com/2022/11/10/H30-5.png" alt=""><figcaption></figcaption></figure>

&#x20;（依Raid卡型号不同，有些可能没有此功能，如没有请跳过此步）提示SSD缓存，选择【Cancel】

<figure><img src="https://pic.chjina.com/2022/11/10/H31-5.png" alt=""><figcaption></figcaption></figure>

&#x20;提示初始化，选择【yes】

<figure><img src="https://pic.chjina.com/2022/11/10/H32-5.png" alt=""><figcaption></figcaption></figure>

&#x20;正在初始化，能看到百分比进度条（速度较快，可能一闪而过）

<figure><img src="https://pic.chjina.com/2022/11/10/H33-5.png" alt=""><figcaption></figcaption></figure>

&#x20;初始化完成！点击【Home】返回首页

<figure><img src="https://pic.chjina.com/2022/11/10/H34-5.png" alt=""><figcaption></figcaption></figure>

&#x20;阵列配置完成！

<figure><img src="https://pic.chjina.com/2022/11/10/H35-5.png" alt=""><figcaption></figcaption></figure>

Raid5状态显示“Optimal”表示正常，Drives显示四块硬盘绿色Online正常

&#x20;最后点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器！

<figure><img src="https://pic.chjina.com/2022/11/10/H36-5.png" alt=""><figcaption></figcaption></figure>

### 6．Raid0的配置

Raid0的配置过程与Raid1大致相同，唯一不同是在选择Raid级别这一步选择Raid0即可。

具体步骤请参考下一节

友情提示：Raid0虽然可以大幅提高读写性能，但是有数据丢失风险，请您慎重考虑！

### 7．Raid1的配置

在WebBIOS主页点击【Configuration Wizard】，打开配置向导

&#x20;选择【Add Configuration】，点击【Next】下一步

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H37x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;选择【Manual Configuration】，点击【Next】下一步

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H38x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;左侧方框内可以看到所有未使用的硬盘。因为要做Raid1，我们选择前两块，然后点击下方的【Add to Array】将其加入到右侧方框内。

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H39x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Accept DG】，创建磁盘组

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H40x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Next】下一步

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H41x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Add to SPAN】，将刚才创建好的磁盘组加入到右侧方框内

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H42x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Next】下一步

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H43x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;阵列参数配置：第一个参数“Raid Level”选择Raid1，其余保持默认

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H44x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;最后一个参数“Select Size”输入阵列容量大小，最大值可参考右侧绿字提示（其中R0代表做Raid0最大容量，R1代表做Raid1最大容量），完成后点击【Accept】

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H45x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;弹出的任何提示均选择【yes】

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H46x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;回到配置页面，点击【Next】下一步

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H47x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;点击【Accept】配置完成！

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H48x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;提示保存，选择【yes】

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H49x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;（依Raid卡型号不同，有些可能没有此功能，如没有请跳过此步）提示SSD缓存，选择【Cancel】

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H50x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;提示初始化，选择【yes】

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H51x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;正在初始化，能看到百分比进度条（速度较快，可能一闪而过）

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H52x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;初始化完成！点击【Home】返回首页

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H53x1m.png" alt=""><figcaption></figcaption></figure>

&#x20;阵列配置完成！

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H54x1m.png" alt=""><figcaption></figcaption></figure>

Raid1状态显示“Optimal”表示正常，Drives显示两块硬盘绿色Online正常，如果还有其它未使用的硬盘，会在unconfigured Drives下面蓝色显示。

&#x20;未使用的硬盘可以继续创建阵列，也可以配置成热备盘。

<figure><img src="http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H55x1m.png" alt=""><figcaption></figcaption></figure>

最后点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器！

### 8．Raid6的配置

在WebBIOS主页点击【Configuration Wizard】，打开配置向导

![img](https://pic.chjina.com/2022/11/10/H56x6m.png)

选择【Add Configuration】，点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H57x6m.png)

选择【Manual Configuration】，点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H58x6m.png)

左侧方框内可以看到所有未使用的硬盘。我们选择全部（也可以逐个选择），然后点击下方的【Add to Array】将其加入到右侧方框内。![img](https://pic.chjina.com/2022/11/10/H59x6m.png)

点击【Accept DG】，创建磁盘组

![img](https://pic.chjina.com/2022/11/10/H60x6m.png)

点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H61x6m.png)

点击【Add to SPAN】，将刚才创建好的磁盘组加入到右侧方框内

![img](https://pic.chjina.com/2022/11/10/H62x6m.png)

点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H63x6m.png)

阵列参数配置：第一个参数“Raid Level”选择Raid6，其余保持默认

![img](https://pic.chjina.com/2022/11/10/H64x6m.png)

最后一个参数“Select Size”输入阵列容量大小，最大值可参考右侧绿字提示（其中R6代表做Raid6的最大容量），完成后点击【Accept】

![img](https://pic.chjina.com/2022/11/10/H65x6m.png)

弹出的任何提示均选择【yes】

![img](https://pic.chjina.com/2022/11/10/H66x6m.png)

回到配置页面，点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H67x6m.png)

点击【Accept】配置完成！

![img](https://pic.chjina.com/2022/11/10/H68x6m.png)

提示保存，选择【yes】

![img](https://pic.chjina.com/2022/11/10/H69x6m.png)

（依Raid卡型号不同，有些可能没有此功能，如没有请跳过此步）提示SSD缓存，选择【Cancel】

![img](https://pic.chjina.com/2022/11/10/H70x6m.png)

提示初始化，选择【yes】

![img](https://pic.chjina.com/2022/11/10/H71x6m.png)

正在初始化，能看到百分比进度条（速度较快，可能一闪而过）

![img](https://pic.chjina.com/2022/11/10/H72x6m.png)

初始化完成！点击【Home】返回首页

![img](https://pic.chjina.com/2022/11/10/H73x6m.png)

阵列配置完成！

Raid6状态显示“Optimal”表示正常，Drives显示四块硬盘绿色Online正常

![img](https://pic.chjina.com/2022/11/10/H74x6m.png)

最后点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器！

### 9．Raid10的配置

在WebBIOS主页点击【Configuration Wizard】，打开配置向导

![img](https://pic.chjina.com/2022/11/10/H75xxm.png)

选择【Add Configuration】，点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H76xxm.png)

选择【Manual Configuration】，点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H77xxm.png)

左侧方框内可以看到所有未使用的硬盘。因为要做Raid10，我们先选择前两块，然后点击下方的【Add to Array】将其加入到右侧方框内。

![img](https://pic.chjina.com/2022/11/10/H78xxm.png)

点击【Accept DG】，创建第一个磁盘组：Drive Group0

![img](https://pic.chjina.com/2022/11/10/H79xxm.png)

然后再选择后两块硬盘，也点击下方的【Add to Array】将其加入到右侧方框内

![img](https://pic.chjina.com/2022/11/10/H80xxm.png)

点击【Accept DG】，创建第二个磁盘组：Drive Group1

![img](https://pic.chjina.com/2022/11/10/H81xxm.png)

点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H82xxm.png)

点击【Add to SPAN】，将刚才创建好的两个磁盘组分别加入到右侧方框内

![img](https://pic.chjina.com/2022/11/10/H83xxm.png)

将第二个磁盘组也添加过来

![img](https://pic.chjina.com/2022/11/10/H84xxm.png)

点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H85xxm.png)

阵列参数配置：第一个参数“Raid Level”选择Raid10，其余保持默认

![img](https://pic.chjina.com/2022/11/10/H86xxm.png)

最后一个参数“Select Size”输入阵列容量大小，最大值可参考右侧绿字提示（其中R10代表做Raid10的最大容量），完成后点击【Accept】

![img](http://www.4008600011.com/wordpress/wp-content/uploads/2017/11/H87xxm.png)

弹出的任何提示均选择【yes】

![img](https://pic.chjina.com/2022/11/10/H88xxm.png)

回到配置页面，点击【Next】下一步

![img](https://pic.chjina.com/2022/11/10/H89xxm.png)

点击【Accept】配置完成！

![img](https://pic.chjina.com/2022/11/10/H90xxm.png)

提示保存，选择【yes】

![img](https://pic.chjina.com/2022/11/10/H91xxm.png)

（依Raid卡型号不同，有些可能没有此功能，如没有请跳过此步）提示SSD缓存，选择【Cancel】

![img](https://pic.chjina.com/2022/11/10/H92xxm.png)

提示初始化，选择【yes】

![img](https://pic.chjina.com/2022/11/10/H93xxm.png)

正在初始化，能看到百分比进度条（速度较快，可能一闪而过）

![img](https://pic.chjina.com/2022/11/10/H94xxm.png)

初始化完成！点击【Home】返回首页

![img](https://pic.chjina.com/2022/11/10/H95xxm.png)

阵列配置完成！Raid10状态显示“Optimal”表示正常，所有硬盘绿色Online正常。最后点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器

![img](https://pic.chjina.com/2022/11/10/H96xxm.png)！

### 10．热备盘（Hotspare）配置

热备盘的作用是如果阵列中有硬盘发生故障，热备盘可以立即顶替，及时将阵列恢复为正常状态。热备盘的配置非常简单，做完阵列后，未使用的硬盘会在WebBIOS中显示为蓝色unconfigured状态，选中该硬盘进入属性页面。

![img](https://pic.chjina.com/2022/11/10/h001hs.png)

选择【Make Global HSP】，点击【GO】执行 （ 此处可以创建两种热备，分别是全局热备Global HSP和专用热备，建议选择全局热备即可 ）

![img](https://pic.chjina.com/2022/11/10/h002hs.png)

配置完成！点击【Home】返回首页

![img](https://pic.chjina.com/2022/11/10/h003hs.png)

热备盘显示为粉色Hotspare状态

![img](https://pic.chjina.com/2022/11/10/h004hs.png)

### 11．控制器属性介绍

点击主界面左侧的“Controller Properties”选项，可以对控制器属性进行查看和更改。

如下是控制器选项的第一个页面，点击“Next”可以进入下一个页面。

![img](https://pic.chjina.com/2022/11/10/h005hs.png)

比较重要的是版本和温度等信息，点击“Next”进入下个页面

![img](https://pic.chjina.com/2022/11/10/h006hs.png)

点击“Next”进入配置界面

![img](https://pic.chjina.com/2022/11/10/h007hs.png)

这个界面中比较常用的是：

Alarm Control：可以配置raid卡的告警声音，Enable是正常告警，Disable是不告警，Silence是本次不告警

点击“Next”进入下一个配置界面

![img](https://pic.chjina.com/2022/11/10/h008hs.png)

比较常用的是：

Emergency Spare：即未配置热备盘时，如果配置了这个选项，则UG也会自动当成是备份盘

点击“Submit”，则会进行配置变更的保存。

## 二、常见问题处理

重要提示：服务器通过Raid技术可以有效增强数据的安全性，但是不代表做了Raid就永远不会出问题，所以数据还是要经常备份的！

一般我们最常遇到的问题就是有硬盘亮红灯了，有些时候还会有报警声。但是请不要担心，硬盘亮红灯不代表硬盘一定有故障，而是硬盘离线了。那么哪些情况会导致硬盘亮红灯呢？ 1、人为拔插过硬盘 2、硬盘没有插到位，接触不良 3、意外停电，影响了阵列信息 4、硬盘发生逻辑上的I/O错误 5、硬盘本身故障

如果您是新机器，硬盘亮红灯大多是因为物流等原因，可能某块硬盘没有插到位，接触不良；如果已经使用了一段时间，大多是因为硬盘发生了逻辑上的I/O错误，因为做了Raid以后，需要多块硬盘协同工作，不仅要把文件打碎，还要一起计算校验值，如果在某一块硬盘上计算错误，可能会导致硬盘被踢出阵列，同时亮红灯报警。如果服务器灰尘较多，容易积蓄静电，也会增加硬盘出错的概率。

下面列举了几个最常见的故障现象，请仔细阅读本手册，5-10分钟即可解决问题！

### 1．一块硬盘显示红色Offline（或者Failed）

进入WebBIOS主页，发现一块硬盘显示红色Offline状态，同时阵列降级变成了蓝色Degraded状态，此时数据还是可用的，选中红色硬盘进入属性页面。

![img](https://pic.chjina.com/2022/11/10/h009fm.png)

在属性列表中找到“Media Error”和“Pred Fail Count”两项（如果找不到请点击【Next】翻页），两项都是零，说明硬盘无故障，可以放心使用！

![img](https://pic.chjina.com/2022/11/10/h010fm.png)

选择【Rebuild Drive】，点击【GO】执行

![img](https://pic.chjina.com/2022/11/10/h011fm.png)

阵列开始同步，能看到百分比进度条，点击【Home】返回首页

![img](https://pic.chjina.com/2022/11/10/h012fm.png)

报错硬盘现在变成了褐色Rebuild状态。如果您着急使用，请点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器，同步过程可以后台进行。我们建议等同步完成再使用，继续查看同步进度请点击左下角【PD Progress Info】

![img](https://pic.chjina.com/2022/11/10/h013fm.png)

查看同步进度

![img](https://pic.chjina.com/2022/11/10/h014fm.png)

### 2．一块硬盘显示红色PD Missing

进入WebBIOS主页，发现一块硬盘显示红色PD Missing状态，同时阵列降级变成了蓝色Degraded状态，此时数据还是可用的，点击【Physical View】进入物理视图。

![img](https://pic.chjina.com/2022/11/10/h015fm.png)

发现一块黑色硬盘显示Foreign Unconfigured Bad状态，选中该硬盘进入属性页面

![img](https://pic.chjina.com/2022/11/10/h016fm.png)

在属性列表中找到“Media Error”和“Pred Fail Count”两项（如果找不到请点击【Next】翻页），两项都是零，说明硬盘无故障，可以放心使用！

![img](https://pic.chjina.com/2022/11/10/h017fm.png)

至此，下面有两种处理办法，都可以解决此问题。（2.2.1和2.2.2任选其一）

**方法1：Clear Foreign Configuration**

选择【Make Unconf Good】，点击【GO】执行

![img](https://pic.chjina.com/2022/11/10/h018fm.png)

点击【Home】返回首页

![img](https://pic.chjina.com/2022/11/10/h019fm.png)

出现一块蓝色硬盘显示Foreign Unconfigured Good状态，点击【Scan Devices】

![img](https://pic.chjina.com/2022/11/10/h020fm.png)

提示发现外来配置信息，选择【Clear】清除

![img](https://pic.chjina.com/2022/11/10/h021fm.png)

提示清除，选择【Yes】

![img](https://pic.chjina.com/2022/11/10/h022fm.png)

回到WebBIOS主页，报错硬盘现在变成了褐色Rebuild状态。如果您着急使用，请点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器，同步过程可以后台进行。我们建议等同步完成再使用，查看同步进度请点击左下角【PD Progress Info】

![img](https://pic.chjina.com/2022/11/10/h023fm.png)

查看同步进度

![img](https://pic.chjina.com/2022/11/10/h024fm.png)

**方法2：Replace Missing PD**

选择【Make Unconf Good】，点击【GO】执行

![img](https://pic.chjina.com/2022/11/10/h025fm.png)

选择【Replace Missing PD】，点击【GO】执行

![img](https://pic.chjina.com/2022/11/10/h026fm.png)

选择【Rebuild Drive】，点击【GO】执行

![img](https://pic.chjina.com/2022/11/10/h027fm.png)

阵列开始同步，能看到百分比进度条，点击【Home】返回首页

![img](https://pic.chjina.com/2022/11/10/h028fm.png)

报错硬盘现在变成了褐色Rebuild状态。如果您着急使用，请点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器，同步过程可以后台进行。我们建议等同步完成再使用，继续查看同步进度请点击左下角【PD Progress Info】

![img](https://pic.chjina.com/2022/11/10/h029fm.png)

查看同步进度

![img](https://pic.chjina.com/2022/11/10/h030fm.png)

### 3．多块硬盘显示红色PD Missing

进入WebBIOS主页，发现多块硬盘显示红色PD Missing状态，阵列已经挂掉变成了红色Offline状态，此时数据已经不可用，点击【Physical View】进入物理视图。

![img](https://pic.chjina.com/2022/11/10/h031fm.png)

发现两块黑色硬盘显示Foreign Unconfigured Bad状态，选中一块硬盘进入属性页面

![img](https://pic.chjina.com/2022/11/10/h032fm.png)

在属性列表中找到“Media Error”和“Pred Fail Count”两项（如果找不到请点击【Next】翻页），两项都是零，说明硬盘无故障，可以放心使用！

![img](https://pic.chjina.com/2022/11/10/h033fm.png)

选择【Make Unconf Good】，点击【GO】执行

![img](https://pic.chjina.com/2022/11/10/h034fm.png)

点击【Home】返回首页

![img](https://pic.chjina.com/2022/11/10/h035fm.png)

已经有一块硬盘变成蓝色Foreign Unconfigured Good状态，同理操作另外一块

![img](https://pic.chjina.com/2022/11/10/h036fm.png)

两块硬盘都变成了蓝色Foreign Unconfigured Good状态，点击【Scan Devices】

![img](https://pic.chjina.com/2022/11/10/h037fm.png)

提示发现外来配置信息，选择【Preview】预览

![img](https://pic.chjina.com/2022/11/10/h038fm.png)

可以看到故障发生前的阵列状态，除了第四块硬盘，其余都是绿色Online正常状态，阵列也恢复为蓝色Degraded（降级状态，此时数据已经恢复可用），点击【Import】导入配置。

![img](https://pic.chjina.com/2022/11/10/h039fm.png)

回到WebBIOS主页，阵列不再是红色Offline损坏状态，第四块硬盘褐色Rebuild状态表示正在恢复阵列至正常状态。请点击【Exit】退出，然后【Ctrl-Alt-Delete】组合键重启服务器，顺利的话系统可以正常启动！修复过程可以后台进行。您也可以等待修复完成后在使用，查看同步进度请点击左下角【PD Progress Info】

![img](https://pic.chjina.com/2022/11/10/h040fm.png)

查看同步进度

![img](https://pic.chjina.com/2022/11/10/h041fm.png)

**引用：**

{% embed url="http://www.4008600011.com/archives/393#4Raid" %}
