# Vcenter 由于一个或多个 vCenter Server 系统的凭据无效，登录失败

## 1. 报错信息

报错内容如下: 由于一个或多个 vCenter Server 系统的凭据无效，登录失败: https://192.168.101.221:443/sdk

![在这里插入图片描述](https://pic.chjina.com/2024/07/03/c987e1d9f6676a11ade3c9cebc47c6db.png)

## 2. 原因

前一天证书快要过期了，重新把证书续订了一遍，但是续订后没有重启服务

## 3. 排除

shell登录后重启vc的服务，

```bash
service-control --stop --all
service-control --start --all
```

或者**直接重启vc**

重新登录恢复正常
