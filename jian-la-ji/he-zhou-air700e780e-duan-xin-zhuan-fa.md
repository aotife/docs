# 合宙Air700E780E短信转发

前几天撸了个合宙的 Air700E开发板用来做短信转发。

* Air700E(只支持移动卡)
* Air780E(支持移动,联通,电信不能收发短信，只上网)
*   Air724UG(支持移动，联通，电信)

    作者的GitHub:[GitHub - 0wQ/air780e-forwarder: Air700E / Air780E / Air780EG 短信转发 (不支持电信卡, Air700E 只支持移动卡)](https://github.com/0wQ/air780e-forwarder)

官方开发板资料

* Air780E : https://doc.openluat.com/wiki/37?wiki\_page\_id=4454
* Air700E : https://doc.openluat.com/wiki/44?wiki\_page\_id=4730
* Air780EG : https://doc.openluat.com/wiki/40?wiki\_page\_id=4588

## 1.下载官方烧录工具

下载地址：https://luatos.com/luatools/download/last

### LuaTools 安装

1、下载[Luatools\_v2.7z](https://luatos.com/luatools/download/last)，解压后是一个文件名为Luatools\_v2.exe的运行程序。 2、新建一个 LuaTools文件夹，将Luatools\_v2.exe拷贝或移动到LuaTools文件夹下 3、双击 Luatools\_v2.exe开始安装，出现如下情况，点击更多信息选项，然后选择仍要运行选项即可完成安装。 ![img](https://pic.chjina.com/2025/09/27/cc8151d8f1378afbc5ba8b35c8af6497.png) 4、运行Luatools后会提示开始升级，点击开始，进行升级，升级完成后可正常使用。

## 2.下载短信转发项目

#### 2.1下载作者的GitHub项目

[GitHub - 0wQ/air780e-forwarder: Air700E / Air780E / Air780EG 短信转发 (不支持电信卡, Air700E 只支持移动卡)](https://github.com/0wQ/air780e-forwarder)

其他项目 https://github.com/hsSam/air780e-forwarder/

下载下来会有两个文件夹

core是存放开发板固件

script是存放开发板脚本

soc后缀的是固件，lua后缀的是脚本

![image-20230409122118382](https://pic.chjina.com/2023/04/9/image-20230409122118382.png)

#### 2.2修改配置文件

用记事本打开script下的config.lua文件，根据文件要求改成自己的接口

![image-20230409123007331](https://pic.chjina.com/2023/04/9/image-20230409123007331.png)

参数解释：

```
 NOTIFY_TYPE = "pushdeer",
```

设置通知的类型，可以设置多个用`,`隔开，下方的配置具体的参数，配置什么通知类型，下面通知参数就填上对应的信息。

设置好详细的参数后保存即可。

## 3.刷写固件和脚本

### 3.1打开官方烧录工具

1. 勾选4G模块USB打印
2.  项目管理测试

    ![image-20230409124036415](https://pic.chjina.com/2023/04/9/image-20230409124036415.png)
3.  点击创建项目

    ![image-20230409124411614](https://pic.chjina.com/2023/04/9/image-20230409124411614.png)
4.  添加固件和脚本文件

    ![image-20230409125001888](https://pic.chjina.com/2023/04/9/image-20230409125001888.png)
5.  点击下载底层和脚本，软件提示请按住BOOT键复位设备.......

    ![image-20230409125258063](https://pic.chjina.com/2023/04/9/image-20230409125258063.png)
6.  按BOOT键和电源键，下图三个按键，从左到右依次是，BOOT键 复位键 电源键 （700E 780E通用按键）

    ![image-20230409125812217](https://pic.chjina.com/2023/04/9/image-20230409125812217.png)
7. 软件提示刷写完成，这时候拔掉数据线插上手机卡可以用了

## 4.其他说明

### 后期想改配置文件

更改config.lua配置文件，在烧录的界面，点击下载脚本就行了，不用再选底层和脚本了

### 设置来电自动启动

默认是通电按电源键启动，万一断电后，忘记按电源键启动了，后期就收不到短信了

方法：短接电源键旁边的空焊盘，可以动一个0欧的电阻或直接用焊锡点上。

![image-20230409132926088](https://pic.chjina.com/2023/04/9/image-20230409132926088.png)
