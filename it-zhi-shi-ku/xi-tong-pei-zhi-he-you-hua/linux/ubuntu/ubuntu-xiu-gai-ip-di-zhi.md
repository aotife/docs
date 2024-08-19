# Ubuntu修改ip地址

Ubuntu 网卡的配置文件在`/etc/netplan/`目录下

## 1.修改为静态地址

一般为`/etc/netplan/50-installer-config.yaml`

```yaml
\# This file is generated from information provided by the datasource.  Changes
\# to it will not persist across an instance reboot.  To disable cloud-init's
\# network configuration capabilities, write a file
\# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
\# network: {config: disabled}
network:
  ethernets:
​    ens33:
​      addresses:
​      - 192.168.144.12/24     //IP地址及掩码
​      nameservers:
​        addresses:
​        - 223.5.5.5          //DNS地址
​        search: []
​      routes:
​      -  to: default
​        via: 192.168.144.2    //网关
  version: 2
```

使用vi 编辑器进行更改

```
sudo vi /etc/netplan/50-cloud-init.yaml

然后输入密码
```

按i键进入编辑模式进行修改，修改完后按键盘的`Esc`推出编辑模式，按`：wq`进行保存。

然后输入sudo netplan apply 使网卡生效

```
sudo netplan apply
```

## 2.如果将配置修改为DHCP

```yaml
# This file is generated from information provided by the datasource. Changes
# to it will not persist across an instance reboot. To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: yes  # 启用DHCP
      # 下面的设置在DHCP模式下将被忽略
      # addresses:
      # - 192.168.144.12/24
      # nameservers:
      #   addresses:
      #   - 223.5.5.5
      # search: []
      routes:
      # 默认路由通常由DHCP服务器提供，因此也可以省略
      # - to: default
      #   via: 192.168.144.2
```

然后输入sudo netplan apply 使网卡生效

```
sudo netplan apply
```
