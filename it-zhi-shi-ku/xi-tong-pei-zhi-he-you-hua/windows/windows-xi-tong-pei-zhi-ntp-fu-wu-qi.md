# Windows系统配置NTP服务器

## 1.开启Windows 时间服务。

* 在桌面上右击“计算机”，选择“管理”，然后选择“服务”。或者按下windows键+R 组合键打开运行，输入： services.msc
* 选中“Windows Time”，设置为开启，这样就可以将“Windows Time”这一个服务打开。

![img](https://pic.chjina.com/2023/03/14/Pasted-1414.png)

## 2.修改注册表

* “开始”--》“运行”--》输入“regedit”打开注册表，在注册表中依次展开：HKEY\_LOCAL\_MACHINE、SYSTEM、CurrentControlSet、Services、W32Time、TimeProviders、NtpServer， 在NtpServer项的右侧键值Enablied，将默认的0改为1，1为启用NTP服务器。

![img](https://pic.chjina.com/2023/03/14/Pasted-1418.png)

![img](https://pic.chjina.com/2023/03/14/Pasted-769.png)

![img](https://pic.chjina.com/2023/03/14/Pasted-1419.png)

* 再在注册表中依次展开：HKEY\_LOCAL\_MACHINE、SYSTEM、CurrentControlSet、Services、W32Time、Config 找到Config项右侧的AnnounceFlags。把默认的10改为5，5的意思就是自身为可靠的时间源。

![img](https://pic.chjina.com/2023/03/14/Pasted-1415.png)

![img](https://pic.chjina.com/2023/03/14/Pasted-1416.png)

## 3.重启NTP服务

* 在命令提示符中输入：net stop w32Time，回车，等待NTP服务停止。
* 然后再输入：net start w32Time，回车，启动NTP服务。

![img](https://pic.chjina.com/2023/03/14/Pasted-1420.png)

## 4.设置防火墙

**NTP服务启动后需要关闭电脑防火墙或者放行入流量UDP 123端口，否则客户端无法正常同步时间信息。**

### 防火墙放通NTP端口(UDP 123)

### 1.点击高级设置。

![image-20230314220958717](https://pic.chjina.com/2023/03/14/image-20230314220958717.png)

### 2.点击入站规则。

![image-20230314221155263](https://pic.chjina.com/2023/03/14/image-20230314221155263.png)

### 3.新建规则

![image-20230314221335006](https://pic.chjina.com/2023/03/14/image-20230314221335006.png)

### 4.选择端口，点击下一步。

![image-20230314221422699](https://pic.chjina.com/2023/03/14/image-20230314221422699.png)

### 5.选择UDP，特定端口输入123，点击下一步。

![image-20230314221504198](https://pic.chjina.com/2023/03/14/image-20230314221504198.png)

### 6.选择允许连接，点击下一步。

![image-20230314221619795](https://pic.chjina.com/2023/03/14/image-20230314221619795.png)

### 7.全部勾选，点击下一步。

![image-20230314221755063](https://pic.chjina.com/2023/03/14/image-20230314221755063.png)

### 8.输入一个你喜爱的名称。点击完成。

![image-20230314221942563](https://pic.chjina.com/2023/03/14/image-20230314221942563.png)

### 9.查看下

![img](https://pic.chjina.com/2023/03/14/Pasted-1421.png)

![img](https://pic.chjina.com/2023/03/14/Pasted-1422.png)
