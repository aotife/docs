# VMware vCenter Server Appliance重置root密码

近期不少用户计划给vCenter升级补丁，但是登录https://vcsa\_fqdn:5480；输入root账户和密码之后报错：

[![img](https://pic.chjina.com/2024/09/23/378515e94697b559bf27959b2d9eeb86.png)](https://www.dinghui.org/wp-content/uploads/2021/04/image-37.png)

root 密码过期时会发生此问题。设备 root 帐户的密码有效期定义为 90 天。如果在有效期内未更改密码，root 用户将无法登录设备管理控制台。（文末也会介绍后续如何规避该问题）

解决办法：

## 修改引导

1、重启VMware vCenter Server Appliance虚拟机，在GNU Grub 界面按 “==e==” , 如下图输入==rw init=/bin/bash==

![img](https://pic.chjina.com/2024/09/23/27e78a4b7557d637d0a2ac7f88cf3408.png)

2、按F10，启动，使用passwd 命令修改root密码

![img](https://www.dinghui.org/wp-content/uploads/2021/04/image-39.png)

3、umount 系统之后，重启。就可以用重置后的密码登陆了。

4、为了规避90天之后又出现同样的无法登录问题，登录https://vcsa\_fqdn:5480；系统管理，密码过期设置，编辑，密码到期修改为否。

\[![img](https://pic.chjina.com/2024/09/23/5435a8dfe31057789aa0b00692bfb2cd.png)

以上是重置密码的方法。

**注意：**

如果你的版本高于vCenter 7.0，而且root密码仅仅忘了配置密码不过期，90天后到期导致无法访问，报错如下：

![img](https://pic.chjina.com/2024/09/23/7b6375f68391af5dd60f09b2a8cfcc82.png)

可以使用如下方法（不用重启vCenter）：

## web修改

1、使用administrator@vsphere.local去登录https://vcsa\_fqdn:5480；

![img](https://pic.chjina.com/2024/09/23/5a70e054f017e216e8f4abccf6427056.png)2、操作，更改root密码，输入原始密码，并输入新密码。

\[![img](https://pic.chjina.com/2024/09/23/6454fa5dfda13e4833beafe15c44c289.png)

更改成功后，再次使用修改后的root密码登录进去将密码到期设置为否。

3、如果web登录修改还不行各种报错的话，用administrator@vsphere.local登录ssh之后，输入shell切换模式，之后使用

sudo passwd root

\[![img](https://pic.chjina.com/2024/09/23/56671796d21f3f5edff387ec3188b840.png)

如果这个还是行不通，拉到最开头的方法，一定可行！

## 命令修改

直接administrator@vsphere.local登陆shell，sudo passwd root就行了！哈哈。实测可以
