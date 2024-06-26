# VMWare ESXi重置root密码

**一、官方方法**

官方KB说明是通过重新安装来重置root密码。

优点：目前6、7和8的版本都支持这种重置方法

缺点：ESXi主机相关的配置信息全部丢失，就剩内部的虚拟机

1、通过ESXi ISO镜像启动服务器，等待到安装界面，按 **回车** 键继续

![220-10.png](https://pic.chjina.com/2024/06/26/a2310494e458f83480719ced6831fbe3.png)

2、按 **F11** 键继续安装

![220-11.png](https://pic.chjina.com/2024/06/26/769c46a2f7fcad64217ad7e2e8c74135.png)

3、选择安装磁盘（需要选择以前老的ESXi安装的磁盘），选好按 **回车** 键继续

![220-12.png](https://pic.chjina.com/2024/06/26/087ed8c50bdd76fcc53eb2cb8ab697d0.png)

4、这步非常重要，由于是覆盖安装因此会提示“ESXi and VMFS Found”并给出三个装模式选项，这边要选择第二个模式，按 **回车** 键继续。

第一个升级ESXi并保留数据文件：Upgrade ESXi，preserve VMFS datastore

第二个安装ESXi并保留数据文件：Install ESXi，preserve VMFS datastore

第三个安装ESXi并覆盖数据文件：Install ESXi，overwrite VMFS datastore

![220-13.png](https://pic.chjina.com/2024/06/26/099f9e014c945d75855acccd496783f6.png)

5、语言默认即可，按 **回车** 键继续

![220-14.png](https://pic.chjina.com/2024/06/26/26995b62e75669378ae90cd3906403e4.png)

6、输入新的密码，按 **回车** 键继续

![220-15.png](https://pic.chjina.com/2024/06/26/8b2359efc83cf02377692770e584bbcf.png)

7、按 **F11** 键开始安装

![220-16.png](https://pic.chjina.com/2024/06/26/8aa3502ba19eced68cb70a617c24b250.png)

8、按 **回车** 键重启，重启完就是新密码了。

![220-17.png](https://pic.chjina.com/2024/06/26/935f555996d8199e8907fac52e9e2682.png)

参考链接：[ESXI忘记密码 重置root密码 - ashenga - 博客园 (cnblogs.com)](https://www.cnblogs.com/itsheng/p/17254578.html)
