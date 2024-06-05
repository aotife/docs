# 创建systemd service服务

## 创建服务

编写 Systemd 服务文件是一种在 Linux 中管理系统服务的标准方法。下面是一个简单的示例，展示了如何编写一个 Systemd 服务文件：

### 1.创建服务文件

首先，使用你喜欢的文本编辑器创建一个新的服务文件。通常情况下，这些文件存储在 `/etc/systemd/system/` 目录下，并以 `.service` 为扩展名。例如，我们可以创建一个名为 `my_service.service` 的文件。

```bash
vi /etc/systemd/system/my_service.service
```

### 2.编写服务文件

在编辑器中，添加以下内容作为你的服务文件模板：

```plaintext
[Unit]
Description=My Custom Service
After=network.target

[Service]
Type=simple
ExecStart= /usr/bin/python3 /path/to/your/script.py
Restart=always

[Install]
WantedBy=multi-user.target
```

* `[Unit]` 部分用于定义单元的属性，如描述和依赖关系。
* `[Service]` 部分用于指定服务的类型、启动命令和重启策略。
* `[Install]` 部分用于指定服务的安装位置，例如在哪个 target 下启用该服务。

确保将 `/path/to/your/script.py` 替换为你的 Python 脚本的实际路径。

### 3.保存文件

保存并退出编辑器。

### 4.重新加载 Systemd 配置

运行以下命令以重新加载 Systemd 配置文件：

```bash
sudo systemctl daemon-reload
```

这样做可以使 Systemd 意识到新的服务文件。

### 5.启动服务

使用以下命令启动你的服务：

```bash
sudo systemctl start my_service.service
```

如果你想要开机自启动，可以使用以下命令：

```bash
sudo systemctl enable my_service.service
```

现在，你的 Python 脚本应该作为一个 Systemd 服务在后台运行了。

## 查看日志

要查看服务的日志，你可以使用 `journalctl` 命令。以下是一些常用的选项：

### 1.**查看特定服务的日志**：

```bash
journalctl -u your_service_name.service
```

将 `your_service_name.service` 替换为你要查看的服务名称。

### 2.**实时查看日志**：

```bash
journalctl -u your_service_name.service -f
```

这会实时显示服务的日志输出。按 `Ctrl + C` 可以退出实时查看。

### 3.**查看最近的日志条目**：

```bash
journalctl -u your_service_name.service --since "1 hour ago"
```

这会显示最近一小时内的日志条目。你可以更改 `1 hour ago` 为其他时间范围，如 `1 day ago`、`30 minutes ago` 等。

### 4.**按照时间戳排序**：

```bash
journalctl -u your_service_name.service --since "1 hour ago" --utc
```

添加 `--utc` 标志可以确保按照 UTC 时间戳排序。

### 5.**以更简洁的格式显示日志**：

```bash
journalctl -u your_service_name.service --since "1 hour ago" --no-pager
```

添加 `--no-pager` 标志可以在终端中显示完整的日志内容，而不是使用分页器显示。

通过这些命令，你可以方便地查看和分析你的服务日志，以便进行故障排除和监视服务的运行情况。
