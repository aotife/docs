# Ubuntu安装ssh

### Ubuntu主机联网

#### 1.更新下软件包

```
sudo apt update
```

#### 2.安装ssh服务端

```
sudo apt-get install openssh-server
```

#### 3.查看ssh服务端运行状态

```
sudo systemctl status ssh
```

#### 4.测试 client能否ssh登录到Ubuntu，如不能连接到Ubuntu，尝试放通Ubuntu防火墙

```
sudo ufw allow ssh
```

### Ubuntu主机不能联网

[点击下载离线安装包](https://drive.chjina.com/ISO/Linux/Ubuntu/deb%E5%8C%85/sshd/)

上传到Ubuntu主机内

安装

```
sudo -dpkg dpkg -i openssh-server.deb
sudo -dpkg dpkg -i openssh-sftp-server.deb 
```
