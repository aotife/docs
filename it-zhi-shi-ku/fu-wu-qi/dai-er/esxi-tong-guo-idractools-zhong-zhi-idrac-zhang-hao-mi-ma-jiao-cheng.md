# ESXi 通过 iDRACTools 重置 iDRAC 账号密码教程

本文适用于 Dell PowerEdge 服务器（支持 iDRAC 7/8/9 系列），通过在 ESXi 主机上安装 iDRACTools 工具包，使用 `racadm` 命令行工具重置 iDRAC 本地账号密码。该方法无需物理接触服务器，仅需获取 ESXi 主机管理权限即可操作。

## 一、 前期准备

### 1.1 必备条件

* 已获取 ESXi 主机的 `root` 账号权限（用于登录 ESXi Shell 或 SSH）；
* ESXi 主机已联网（用于下载 iDRACTools 工具包，离线环境可提前下载后上传）；
* 确认服务器为 Dell PowerEdge 系列，且搭载可正常工作的 iDRAC 控制器；
* 新密码需符合 iDRAC 密码策略：长度≥8 位，包含大写字母、小写字母、数字及特殊符号（如 `@、#、$` 等）。

### 1.2 工具下载

下载对应 ESXi 版本的 Dell EMC iDRACTools VIB 包，下载地址：[https://www.dell.com/support/drivers](https://www.dell.com/support/drivers)

{% stepper %}
{% step %}
### 在下载中心搜索服务器型号

示例：PowerEdge R750、R640 等。
{% endstep %}

{% step %}
### 选择操作系统

选择“操作系统”为对应 ESXi 版本（如 ESXi 6.7、ESXi 7.0）。
{% endstep %}

{% step %}
### 下载 Dell EMC iDRAC Tools VIB 包

找到“Dell EMC iDRAC Tools”分类，下载后缀为 `.zip` 的 VIB 包（如 `DellEMC-iDRACTools-Web-ESX67i.VIB-10.1.0.0-4568_A00.zip`）。
{% endstep %}
{% endstepper %}

## 二、 启用 ESXi SSH 服务（可选）

若通过远程 SSH 登录 ESXi 主机，需先启用 SSH 服务；若直接在服务器本地操作 ESXi Shell，可跳过此步骤。

{% stepper %}
{% step %}
### 在 vSphere / ESXi 管理界面打开主机服务

登录 ESXi 管理界面或在 vCenter 上打开 ESXi 的 SSH 服务。
{% endstep %}

{% step %}
### 导航至服务管理

点击左侧“主机” → 右侧“管理” → “服务”。
{% endstep %}

{% step %}
### 启动 Secure Shell (SSH)

在服务列表中找到“Secure Shell (SSH)”，点击“启动”，并可设置“策略”为“自动启动”（避免重启后失效）。
{% endstep %}
{% endstepper %}

## 三、 上传并安装 iDRACTools 工具包

### 3.1 上传 VIB 包到 ESXi 主机

使用文件传输工具（如 WinSCP、FileZilla）连接 ESXi 主机（用户：root，密码：ESXi root 密码），将下载的 iDRACTools VIB 包上传至 ESXi 主机的临时目录（如 `/tmp`）或数据存储目录。

### 3.2 安装 iDRACTools

{% stepper %}
{% step %}
### 登录 ESXi Shell 或通过 SSH 远程连接

远程 SSH 连接（使用 PuTTY 等工具，输入 ESXi 主机 IP，用户名 root）或直接在服务器本地打开 ESXi Shell。
{% endstep %}

{% step %}
### 切换到 VIB 包所在目录

示例（以上传至 `/tmp` 为例）：

```bash
cd /tmp
```
{% endstep %}

{% step %}
### 执行安装命令

（需指定 VIB 包完整文件名，建议直接复制文件名避免输入错误）：

```bash
esxcli software vib install -d /tmp/DellEMC-iDRACTools-Web-ESX67i.VIB-10.1.0.0-4568_A00.zip
```
{% endstep %}

{% step %}
### 验证安装结果

若输出 `Operation finished successfully` 且 `Reboot Required: false`，说明安装成功，无需重启 ESXi 主机。
{% endstep %}

{% step %}
### 确认 racadm 工具可用

执行：

```bash
racadm help
```

若输出 `racadm` 命令帮助信息，说明工具已正常加载。
{% endstep %}
{% endstepper %}

## 四、 重置 iDRAC 账号密码

### 4.1 确认 iDRAC 用户索引

iDRAC 每个用户对应唯一索引（1\~16），默认管理员账号（通常为 root）的索引为 2。可通过以下命令查看所有用户索引及基本信息：

```bash
racadm get iDRAC.Users
```

输出结果中，`iDRAC.Users.2` 即为默认管理员账号（可通过 `UserName` 字段确认账号名称）。

### 4.2 执行密码重置命令

使用以下命令重置指定索引用户的密码（以重置索引 2 的默认管理员账号为例）：

```bash
racadm set iDRAC.Users.2.Password 新密码
```

示例（设置密码为 P@ssw0rd）：

```bash
racadm set iDRAC.Users.2.Password P@ssw0rd
```

注意：若修改非默认用户密码，需将命令中的“2”替换为对应用户的索引（如重置索引 3 的用户密码，命令为 `racadm set iDRAC.Users.3.Password 新密码`）。

### 4.3 确认重置成功

若命令执行后输出 `Object value modified successfully`，说明密码已重置成功。

## 五、 验证密码有效性

通过 iDRAC 网页界面验证新密码是否可用：

{% stepper %}
{% step %}
### 查看 iDRAC IP 地址

可通过以下命令查看 iDRAC 的 IP：

```bash
racadm get iDRAC.NIC.IPv4Address
```
{% endstep %}

{% step %}
### 登录 iDRAC 网页

在浏览器中输入 iDRAC 控制器的 IP 地址，进入 iDRAC 登录页面，输入重置后的账号（如 root）和新密码。
{% endstep %}

{% step %}
### 排查登录失败

若能成功登录，说明密码重置生效；若登录失败，可重新执行 4.2 步骤检查命令是否正确，或确认用户索引是否匹配。
{% endstep %}
{% endstepper %}

## 六、 后续操作（可选）

### 6.1 配置 iDRAC 网络地址（可选）

若需设置或修改 iDRAC 的网络地址（静态 IP/动态 IP），可通过以下 `racadm` 命令操作，无需进入 iDRAC 网页界面：

1. 查看当前 iDRAC 网络配置（确认现有 IP、DHCP 状态等）：

```bash
racadm get iDRAC.NIC
```

2. 启用 DHCP（自动获取 IP，适合动态网络环境）：

```bash
racadm set iDRAC.NIC.DHCPEnable 1
```

3. 禁用 DHCP，设置静态 IP（适合固定管理地址场景，需替换为实际网络参数）：

```bash
# 禁用 DHCP
racadm set iDRAC.NIC.DHCPEnable 0

# 设置静态 IP 地址
racadm set iDRAC.NIC.IPv4Address 192.168.1.200

# 设置子网掩码
racadm set iDRAC.NIC.IPv4Netmask 255.255.255.0

# 设置网关地址
racadm set iDRAC.NIC.IPv4Gateway 192.168.1.1

# （可选）设置首选 DNS 服务器
racadm set iDRAC.NIC.DNS1 8.8.8.8

# （可选）设置备用 DNS 服务器
racadm set iDRAC.NIC.DNS2 8.8.4.4
```

注意：修改 iDRAC 网络配置后，需执行 `racadm racreset` 重启 iDRAC 才能生效，重启期间 iDRAC 暂时无法访问，约 1\~2 分钟后恢复。

### 6.2 重启 iDRAC（若需）

部分情况下，修改密码后需重启 iDRAC 才能完全生效，重启命令（不影响 ESXi 主机运行）：

```bash
racadm racreset
```

重启过程约 1\~2 分钟，期间无法访问 iDRAC，重启完成后即可正常使用。

### 6.3 清理临时文件

若 VIB 包上传至 `/tmp` 目录，可删除安装包节省空间：

```bash
rm -rf /tmp/DellEMC-iDRACTools-Web-ESX67i.VIB-10.1.0.0-4568_A00.zip
```

## 七、 常见问题排查

<details>

<summary>racadm: command not found</summary>

原因是 iDRACTools 未安装成功或未加载。解决方案：重新执行安装命令，检查 VIB 包文件名是否正确，或重启 ESXi 主机后再次尝试。

</details>

<details>

<summary>Object value modified successfully 但登录失败</summary>

原因可能是用户索引错误或密码不符合策略。解决方案：通过 `racadm get iDRAC.Users` 确认正确索引，重新设置符合要求的强密码。

</details>

<details>

<summary>安装 VIB 包提示依赖错误</summary>

原因是 VIB 包版本与 ESXi 版本不匹配。解决方案：重新下载对应 ESXi 版本的 iDRACTools 包。

</details>
