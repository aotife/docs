# CentOS防火墙

### firewalld 防火墙状态命令

命令：`systemctl status firewalld`

执行上述命令，即可查看当前防火墙的状态。 如果防火墙的状态参数Active是active (running)，则防火墙为开启状态。如果防火墙的状态参数是inactive (dead)，则防火墙为关闭状态。 实例：

```cmd
[root@localhost ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
     Docs: man:firewalld(1)
```

上述例子中防火墙Active为inactive (dead)，所以防火墙处于关闭状态。

```cmd
[root@localhost ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2023-07-04 18:06:58 CST; 5s ago
     Docs: man:firewalld(1)
 Main PID: 16208 (firewalld)
   CGroup: /system.slice/firewalld.service
           └─16208 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

Jul 04 18:06:58 localhost.localdomain systemd[1]: Starting firewalld - dynamic firewall daemon...
Jul 04 18:06:58 localhost.localdomain systemd[1]: Started firewalld - dynamic firewall daemon.
Jul 04 18:06:58 localhost.localdomain firewalld[16208]: WARNING: AllowZoneDrifting is enabled. This is considered...now.
Hint: Some lines were ellipsized, use -l to show in full.
```

上述例子中防火墙Active为inactive (running)，所以防火墙处于开启状态。

### 查看开放规则

命令`sudo firewall-cmd --list-all`

```cmd
[root@localhost ~]# sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0 eth1
  sources: 
  services: dhcpv6-client ssh
  ports: 443/tcp 19988/tcp 26084/tcp 7800/tcp 35601/tcp 35601/udp 8008/tcp 5555/tcp 8181/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

上述例子中可以看到当前的防火墙配置如下：

* 区域（Zone）：public（活动状态）
* 默认目标（Target）：default
* ICMP反转（icmp-block-inversion）：未启用
* 接口（Interfaces）：eth0、eth1
* 源地址（Sources）：无
* 服务（Services）：dhcpv6-client、ssh
* 端口（Ports）：443/tcp、19988/tcp、26084/tcp、7800/tcp、35601/tcp、35601/udp、8008/tcp、5555/tcp、8181/tcp
* 协议（Protocols）：无
* 伪装（Masquerade）：未启用
* 转发端口（Forward-ports）：无
* 源端口（Source-ports）：无
* ICMP阻塞（ICMP-blocks）：无
* Rich规则（Rich rules）：无

这些配置表示在public区域中，允许通过防火墙的服务有dhcpv6-client和ssh，允许通过的端口有443/tcp、19988/tcp、26084/tcp、7800/tcp、35601/tcp、35601/udp、8008/tcp、5555/tcp、8181/tcp。

### 放通(关闭)端口

参数说明：

firewall-cmd:是linux提供的操作firewall的一个工具

\--permanent:将规则永久性地添加到防火墙配置中，使其在系统重启后保持有效。

\--add-port:从防火墙规则中添加指定的端口

\--remove-port=:从防火墙规则中移除指定的端口

\--add-service= : 从防火墙规则中添加指定的服务。

\--remove-service=: 从防火墙规则中移除指定的服务。

\--reload: 重新加载防火墙配置，以使新的规则生效

#### 放开端口

开放445端口，可以使用以下命令：

```
sudo firewall-cmd --zone=public --add-port=445/tcp --permanent
```

这将在public区域中永久允许TCP协议的445端口通过防火墙。请确保在执行此命令之前已经了解了相关安全风险，并且只在必要时开放端口。

执行完上述命令后，使用`--reload`选项重新加载防火墙配置，使更改生效：

```
sudo firewall-cmd --reload
```

这样就成功开放了445端口。

#### 关闭端口

关闭445端口并阻止通过防火墙，

1.  使用以下命令将445端口从防火墙规则中删除：

    ```
    sudo firewall-cmd --zone=public --remove-port=445/tcp --permanent
    ```

    这将从防火墙规则中删除TCP协议的445端口。
2.  重新加载防火墙配置以使更改生效：

    ```
    sudo firewall-cmd --reload
    ```

    现在，445端口将被防火墙阻止，并且无法通过该端口进行连接。

### 放通(关闭)服务

#### 开放服务

开放SSH服务并允许通过防火墙，

1.  确保防火墙允许SSH服务通过。使用命令添加SSH服务到防火墙规则：

    ```
    sudo firewall-cmd --zone=public --add-service=ssh --permanent
    ```

    这将在防火墙规则中添加SSH服务。
2.  执行完上述命令后，使用`--reload`选项重新加载防火墙配置，使更改生效：

    ```
    sudo firewall-cmd --reload
    ```

    现在，SSH服务将通过防火墙，并且您应该能够通过SSH远程连接到系统。

#### 关闭服务

关闭SSH服务并阻止通过防火墙，请按照以下步骤进行操作：

1.  使用以下命令将SSH服务从防火墙规则中删除：

    ```
    sudo firewall-cmd --zone=public --remove-service=ssh --permanent
    ```

    这将从防火墙规则中删除SSH服务。
2.  重新加载防火墙配置以使更改生效：

    ```
    sudo firewall-cmd --reload
    ```

    现在，SSH服务将被防火墙阻止，并且无法通过SSH进行远程连接。

### 查询端口

查询8080端口是否开放

`firewall-cmd --query-port=8080/tcp`

```
[root@185 ~]# firewall-cmd --query-port=8080/tcp
no
```

返回no表示没有开放

返回yes表示已经开放

### 开启或关闭防火墙命令

* 开启命令：`systemctl start firewalld`
* 临时关闭命令：`systemctl stop firewalld`（服务器重启后会重新启动）
* 永久关闭命令：`systemctl disable firewalld`（服务器重启后会不会启动）
* 查看防火墙状态命令：\`\`systemctl status firewalld
