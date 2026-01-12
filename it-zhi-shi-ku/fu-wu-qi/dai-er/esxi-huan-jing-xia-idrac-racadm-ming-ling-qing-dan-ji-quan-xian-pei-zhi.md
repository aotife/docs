# ESXi 环境下 iDRAC racadm 命令清单及权限配置

该清单基于 Dell EMC iDRAC Tools VIB 安装完成后的操作，适用于 ESXi 主机本地 Shell/SSH 环境，可快速管理 iDRAC 配置。

## 一、 用户管理命令

### 查看用户列表及详情

```bash
# 列出所有 iDRAC 用户的基本信息
racadm get iDRAC.Users

# 查看指定用户（索引为2）的完整配置（用户名、权限、状态等）
racadm get iDRAC.Users.2
```

### 修改用户密码

```bash
# 设置用户2的密码（替换为实际密码）
racadm set iDRAC.Users.2.Password "YourStrongPassword@123"
```

### 启用/禁用用户

```bash
# 启用用户2
racadm set iDRAC.Users.2.Enabled 1

# 禁用用户2
racadm set iDRAC.Users.2.Enabled 0
```

### 设置用户权限

iDRAC 用户权限用“权限位数值”表示，常见权限组合：

* `0`：无权限
* `4`：只读权限
* `15`：管理员权限（完整控制）

```bash
# 设置用户2为管理员权限
racadm set iDRAC.Users.2.Privilege 15
```

### 创建新用户（适用于 iDRAC 7/8/9）

{% stepper %}
{% step %}
### 启用用户槽位

```bash
# 启用用户槽位3
racadm set iDRAC.Users.3.Enabled 1
```
{% endstep %}

{% step %}
### 设置用户名

```bash
# 设置用户名
racadm set iDRAC.Users.3.UserName "new_admin"
```
{% endstep %}

{% step %}
### 设置密码

```bash
# 设置密码
racadm set iDRAC.Users.3.Password "NewPass@456"
```
{% endstep %}

{% step %}
### 设置管理员权限

```bash
# 设置管理员权限
racadm set iDRAC.Users.3.Privilege 15
```
{% endstep %}
{% endstepper %}

***

## 二、 网络配置命令

### 查看 iDRAC 网络信息

```bash
# 查看完整网络配置（IP地址、子网掩码、网关、DHCP状态等）
racadm get iDRAC.NIC

# 仅查看 IP 地址相关信息
racadm get iDRAC.NIC.IPv4Address
```

### 设置静态 IP/动态 IP

```bash
# 启用 DHCP（自动获取 IP）
racadm set iDRAC.NIC.DHCPEnable 1

# 禁用 DHCP，设置静态 IP
racadm set iDRAC.NIC.DHCPEnable 0
racadm set iDRAC.NIC.IPv4Address 192.168.1.100
racadm set iDRAC.NIC.IPv4Netmask 255.255.255.0
racadm set iDRAC.NIC.IPv4Gateway 192.168.1.1
```

### 查看/设置 DNS 配置

```bash
# 查看 DNS 服务器地址
racadm get iDRAC.NIC.DNS1

# 设置首选 DNS 服务器
racadm set iDRAC.NIC.DNS1 8.8.8.8

# 设置备用 DNS 服务器
racadm set iDRAC.NIC.DNS2 8.8.4.4
```

***

## 三、 系统状态与日志命令

### 查看服务器硬件健康状态

```bash
# 查看整体硬件健康摘要
racadm get system.health

# 查看指定组件（如 CPU、内存、磁盘）健康状态
racadm get system.health.cpu
racadm get system.health.storage
```

### 查看/导出 iDRAC 日志

```bash
# 查看最近的 iDRAC 系统事件日志（SEL）
racadm sel view

# 导出 SEL 日志到本地文件（ESXi 主机存储路径）
racadm sel export -f /tmp/idrac_sel.log
```

### 清除 iDRAC 日志

```bash
# 清除系统事件日志（SEL）
racadm sel clear

# 清除 iDRAC 审计日志
racadm clearlog audit
```

***

## 四、 服务器电源控制命令

{% hint style="warning" %}
电源控制会直接影响服务器运行，生产环境请谨慎操作！
{% endhint %}

```bash
# 查看服务器电源状态（On/Off）
racadm get system.powerstate

# 软关机（正常关闭操作系统，类似手动关机）
racadm serveraction powerdown

# 强制关机（直接切断电源，类似拔电源）
racadm serveraction poweroff

# 开机
racadm serveraction powerup

# 重启服务器（先关机再开机）
racadm serveraction powercycle

# 重置服务器（强制重启，不经过正常关机流程）
racadm serveraction hardreset
```

***

## 五、 iDRAC 自身配置命令

### 查看 iDRAC 版本信息

```bash
# 查看 iDRAC 固件版本、型号等信息
racadm get idrac.info
```

### 重置 iDRAC 配置

```bash
# 重置 iDRAC 到出厂默认设置（保留网络配置）
racadm racresetcfg

# 强制重启 iDRAC服务（不影响服务器运行）
racadm racreset
```

### 查看/启用 iDRAC 服务

```bash
# 查看 SSH/HTTP/HTTPS 服务状态
racadm get iDRAC.SSH.Enable
racadm get iDRAC.HTTPS.Enable

# 启用 iDRAC SSH 访问
racadm set iDRAC.SSH.Enable 1

# 启用 iDRAC HTTPS 访问
racadm set iDRAC.HTTPS.Enable 1
```

***

## 六、 命令使用注意事项

{% stepper %}
{% step %}
### 以 root 用户执行

所有命令需在 ESXi Shell 中以 root 用户执行。
{% endstep %}

{% step %}
### 修改后重启 iDRAC

修改配置后，部分设置（如网络、权限）需重启 iDRAC 生效（执行 `racadm racreset`）。
{% endstep %}

{% step %}
### 密码策略

密码设置需符合 iDRAC 密码策略（默认要求大小写字母 + 数字 + 特殊符号，长度≥8位）。
{% endstep %}
{% endstepper %}

***

<details>

<summary>需要我帮你整理一份 iDRAC 权限位数值对照表，方便你精准配置用户权限吗？</summary>

（注：文档部分内容可能由 AI 生成）

</details>
