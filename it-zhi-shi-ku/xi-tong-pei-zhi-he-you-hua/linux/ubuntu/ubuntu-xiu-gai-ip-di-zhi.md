# Ubuntu修改ip地址

Ubuntu 网卡的配置文件在`/etc/netplan/`目录下

## 1.修改为静态地址

一般为`/etc/netplan/50-installer-config.yaml`

```yaml
# This file is generated from information provided by the datasource.  Changes
# to it will not persist across an instance reboot.  To disable cloud-init's
# network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
  version: 2
  renderer: networkd  # 使用 systemd-networkd 管理网络
  ethernets:
    ens33:
      dhcp4: false    # 显式关闭 DHCP
      addresses:
        - 192.168.144.12/24  # 静态 IPv4 地址
      nameservers:
        addresses: [223.5.5.5]  # DNS 服务器（阿里 DNS）
        search: []      # 空搜索域列表
      routes:
        - to: default
          via: 192.168.144.2  # 默认网关

```

使用vi 编辑器进行更改

```
sudo vi /etc/netplan/50-cloud-init.yaml

然后输入密码
```

按i键进入编辑模式进行修改，修改完后按键盘的`Esc`推出编辑模式，按`：wq`进行保存。

### YAML语法有非常严格的格式

1. **格式标准化**
   * 所有缩进统一为 **2 个空格**（YAML 标准）
   * 删除冗余的垂直空行，保持结构紧凑
   * 添加简洁注释（可选，用于解释关键配置）

#### **验证与应用步骤**

**检查语法合法性**

```bash
sudo netplan generate  # 无输出表示语法正确
```

然后输入sudo netplan apply 使网卡生效

```bash
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
