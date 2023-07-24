# Windows 远程提示函数不受支持

**注意：修改远程的电脑，不是被远程的电脑。**



## 一.正常可以打开组策略

### 1、win +R弹出运行窗口输入gpedit.msc，打开计算机组策略配置窗口

![image-20221009090406280](https://pic.chjina.com/2022/10/9/1.png)

### 2.点击计算机配置=>管理模板 =>系统

![image-20221009090521404](https://pic.chjina.com/2022/10/9/2.png)

### 3.在系统设置下找到凭据分配。

![image-20221009090900365](https://pic.chjina.com/2022/10/9/03.png)

### 4.点击凭据分配中的加密数据库修正。

![image-20221009090952670](https://pic.chjina.com/2022/10/9/4.png)

### 5.修改为已启用，保护级别为易受攻击。

![image-20221009091146782](https://pic.chjina.com/2022/10/9/5.png)

### 6.重新远程下连接电脑就可以了。

### 7.如果还是不行，可以把被远程的电脑下面的钩去掉试试。

![image-20230228091929798](https://pic.chjina.com/2023/02/28/image-20230228091929798.png)

## 二. 针对于家庭中文版的没有组策略方法

注：由于作者没有家庭中文版的系统，没有具体实际测试

### 1.win +R弹出运行窗口输入regedit，打开计算机注册表配置窗口

![image-20221009093307625](https://pic.chjina.com/2022/10/9/image-20221009093307625.png)

### 2打开

```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
```

![image-20221009093413134](https://pic.chjina.com/2022/10/9/image-20221009093413134.png)

### 3.新建\CredSSP\Parameters项

![image-20221009094954199](https://pic.chjina.com/2022/10/9/image-20221009094954199.png)

### 4.点开Parameters，在右侧右键鼠标，新建，选择DWORD(32位)值(D)

![image-20221009100452406](https://pic.chjina.com/2022/10/9/image-20221009100452406.png)

### 5.修改数值名称为AllowEncryptionOracle，数值数据改为2

![image-20221009100607355](https://pic.chjina.com/2022/10/9/image-20221009100607355.png)

### 6.点击确定，应该就可以正常远程桌面了。
