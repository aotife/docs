# CentOS重置密码

## 1.重启系统，开机引导界面按 `E`键

![image-20221008092004913](https://pic.chjina.com/2022/10/08/image-20221008092004913.png)

## 2.进入内核编辑界面

![image-20221008092236063](https://pic.chjina.com/2022/10/08/image-20221008092236063.png)

## 3.按方向“↓”键，往下翻到 linux16 这一行，然后在最后加上 `rd.break`

![image-20221008092938274](https://pic.chjina.com/2022/10/08/image-20221008092938274.png)

## 4.`ctrl + x`保存，将进入 Initramfs 的debug 命令模式

![image-20221008093119723](https://pic.chjina.com/2022/10/08/image-20221008093119723.png)

## 5.输入`mount -o remount,rw /sysroot/` 为/sysroot提供读写权限

![image-20221008093740450](https://pic.chjina.com/2022/10/08/image-20221008093740450.png)

## 6.切换至chroot模式

![image-20221008094020967](https://pic.chjina.com/2022/10/08/image-20221008094020967.png)

## 7.输入 `passwd root` 重置root账户的密码

![image-20221008094155253](https://pic.chjina.com/2022/10/08/image-20221008094155253.png)

## 8.输入两遍新的密码，注：输入的时候屏幕不显示字符，

![image-20221008094450877](https://pic.chjina.com/2022/10/08/image-20221008094450877.png)

## 9.执行 `touch /.autorelabel`命令

![image-20221008094718870](https://pic.chjina.com/2022/10/08/image-20221008094718870.png)

## 10.输入exit 退出chroot模式

![image-20221008094814789](https://pic.chjina.com/2022/10/08/image-20221008094814789.png)

## 11.输入exit 推出Initramfs模式，会自动重启,输入新的密码就可以正常登录了

![image-20221008094841220](https://pic.chjina.com/2022/10/08/image-20221008094841220.png)

## 12.完整操作流程

![image-20221008094903681](https://pic.chjina.com/2022/10/08/image-20221008094903681.png)

## 重启后，输入新的密码就可以登录系统。
