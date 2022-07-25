# VCSA6.7无DNS安装

主要安装注意事项：

## 第一阶段：

1. FQDN：VCSA的IP地址
2. IP地址：VCSA的IP地址
3. 子网掩码：VCSA的IP的掩码
4. 网关：VCSA的IP网关
5. DNS：VCSA的IP地址

**FQDN IP地址 DNS** 都是 VCSA的同一个地址

**第一阶段部署完，千万不要点击继续，用浏览器打开5480端口进行部署。**

## 第二阶段

1. 浏览器打开VCSA的5480端口，系统名称`photon-machine`改为 VCSA的地址，打开下面的SSH开关。
2. 点击下一步，这个时候会报错，提示无法保存主机名。
3. 用Xshell 或其他ssh工具，连接VCSA,修改**dnsmasq.conf**的配置文件，让VCSA自己做DNS服务器。

### 修改VCSA的配置文件

1.备份文件

```
cp /etc/dnsmasq.conf /etc/dnsmasq-bak.conf
```

2.修改文件

```
vi /etc/dnsmasq.conf 
```

按i进入编辑模式

```
将no-hosts改为addn-hosts=/etc/dns_add_hosts
将listen-address=127.0.0.1改为listen-address=192.169.1.22
```

192.168.1.22 为VCSA的地址

按Esc键退出编辑，然后输入\*\*:wq\*\*保存文件

3.添加解析记录

```
vi /etc/dns_add_hosts 
```

按i进入编辑模式，输入

```
192.169.1.22 192.169.1.22
192.169.1.22 vcenter   //如果纯IP访问的可以不加
```

按Esc键退出编辑，然后输入\*\*:wq\*\*保存文件

4.重启dnsmasq服务

```
systemctl restart dnsmasq
```

5.测试正反向解析

```
nslookup vcenter  
nslookup 192.169.1.22
```

有解析，返回网页的5480点击下一步，愉快的安装吧。



参考链接：

{% embed url="https://www.cnblogs.com/itfat/p/15234566.html" %}
