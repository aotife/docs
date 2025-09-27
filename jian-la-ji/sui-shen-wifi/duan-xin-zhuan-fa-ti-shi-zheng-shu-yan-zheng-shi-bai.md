# 短信转发提示证书验证失败

用高通随身WIFI安装短信转发 [GitHub](https://github.com/pppscn/SmsForwarder/)应用，设置通道时，提示证书验证失败。

![img](https://pic.chjina.com/2025/09/27/7f2bc2e4217fddf7cc605211dbd74efb.png)

### **替换随身WIFI系统证书，需要Root**

使用RE文件管理器，替换新的的根证书到老旧设备的 /system/etc/security/cacerts/内，**修改权限为644后重启**。

```
/system/etc/security/cacerts
```

新的的根证书可以从新版本的安卓手机中提取。也可以用网友提供的证书。

网友提供证书下载链接，以下证书随意替换一个即可。

1.https://github.com/user-attachments/files/20394975/cacerts.zip

2.https://cooanaoo.lanzoub.com/ir69E33kq84f

3.备份下载

https://drive.chjina.com/%E6%8D%A1%E5%9E%83%E5%9C%BE/%E9%9A%8F%E8%BA%ABWIFI/%E9%AB%98%E9%80%9A%E9%9A%8F%E8%BA%ABWFI%E7%9F%AD%E4%BF%A1%E8%BD%AC%E5%8F%91%E8%AF%81%E4%B9%A6%E9%AA%8C%E8%AF%81%E5%A4%B1%E8%B4%A5/%E8%AF%81%E4%B9%A6

![image-20250927082715425](https://pic.chjina.com/2025/09/27/bd12f05b0cad4fc9d96910f91ed462bd.png)
