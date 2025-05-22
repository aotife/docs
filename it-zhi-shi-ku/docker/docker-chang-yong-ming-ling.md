# Docker常用命令

### 修改Docker 镜像源

#### 1.编辑一下文件

```
sudo vi /etc/docker/daemon.json
```

#### 2.修改成一下配置

```
{
  "registry-mirrors": [
    "你自己的加速地址",
    "https://dh.chjina.com",
    "https://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}
```

#### 3.重启docker

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### Docker CLI详解

例：

```
docker run -d --restart=unless-stopped -v /etc/alist:/opt/alist/data -p 5244:5244 -e PUID=0 -e PGID=0 -e UMASK=022 --name="alist" alist666/alist:latest
```

#### 核心结构

```
docker run [参数] 镜像名:标签
```

表示启动一个新的 Docker 容器，使用 `alist666/alist:latest` 镜像的最新版本。

#### 参数详解

1.  **后台运行模式**

    ```bash
    -d
    ```

    * 以守护进程（detached）模式运行容器，即容器在后台运行。
2.  **自动重启策略**

    ```
    --restart=unless-stopped
    ```

    * 容器异常退出时会自动重启（如程序崩溃、系统重启等）。
    * 如果用户手动停止容器（`docker stop`），则不会自动重启。
3.  **数据卷映射**

    ```
    -v /etc/alist:/opt/alist/data
    ```

    * 前面是宿主机的目录。后面是容器的目录一般修改前面的宿主机的目录即可，修改容器的目录可能会无法启动。将宿主机的 `/etc/alist` 目录挂载到容器的 `/opt/alist/data`。
    * **作用**：配置文件和数据持久化（重启容器不丢失数据）。
    * **注意**：需确保宿主机 `/etc/alist` 目录存在，否则会自动创建空目录。
4.  **端口映射**

    ```
    -p 5244:5244
    ```

    * 将宿主机的 5244 端口映射到容器的 5244 端口。
    * **访问方式**：通过 `http://宿主机IP:5244` 访问 Alist 服务。
5.  **用户权限配置**

    ```
    -e PUID=0 -e PGID=0
    ```

    * 设置容器内进程的运行用户权限。
    * `PUID=0` 和 `PGID=0` 表示以 root 用户运行（0 是 root 的 ID）。
    * **安全问题**：非容器作者推荐不建议使用此命令
6.  **文件权限掩码**

    ```
    -e UMASK=022
    ```

    * 控制新建文件的默认权限（UNIX 权限掩码）。
    * `022` 表示文件权限为 `755`（目录）和 `644`（文件）。
7.  **容器命名**

    ```
    --name="alist"
    ```

    * 将容器命名为 `alist`，便于通过名称管理容器（如 `docker stop alist`）。

#### 镜像信息

```
alist666/alist:latest
```

* 使用 Docker Hub 上用户 `alist666` 维护的 Alist 镜像。
* `latest` 标签表示使用最新版本（生产环境建议指定稳定版本号）。

### Docker Compose 详解

```
version: '3.3'                 # 指定 Docker Compose 版本
services:                     # 定义服务列表
  alist:                      # 服务名称（自定义）
    image: 'alist666/alist:beta' # 指定镜像（此处为 beta 测试版）
    container_name: alist     # 容器命名（覆盖默认生成的名称）
    volumes:                  # 数据卷配置
      - '/etc/alist:/opt/alist/data'  # 宿主机目录映射，前面的是宿主机，后面是容器内
    ports:                    # 端口映射
      - '5244:5244'           # 宿主机端口:容器端口
    environment:              # 环境变量
      - PUID=0                # 用户权限
      - PGID=0                # 用户组权限
      - UMASK=022             # 文件权限掩码
    restart: unless-stopped   # 重启策略 容器异常退出时会自动重启（如程序崩溃、系统重启等）手动
```

### Docker 常用命令

```
查看运⾏中的容器
sudo docker ps

查看所有容器
docker ps -a

搜索镜像
docker search nginx

下载镜像
docker pull nginx

下载指定版本镜像
docker pull nginx:1.26.0

查看所有镜像
docker images

删除指定id的镜像
docker rmi e784f4560448

运⾏⼀个新容器
docker run nginx

停⽌容器
docker stop keen_blackwell

启动容器
docker start 592

重启容器
docker restart 592

查看容器资源占⽤情况
docker stats 592

查看容器⽇志
docker logs 592

删除指定容器
docker rm 592

强制删除指定容器
docker rm -f 592
 
后台启动容器
docker run -d --name mynginx nginx
 
后台启动并暴露端⼝
docker run -d --name mynginx -p 80:80 nginx
 
进⼊容器内部
docker exec -it mynginx /bin/bash
 
提交容器变化打成⼀个新的镜像
docker commit -m "update index.html" mynginx mynginx:v1.0
 
保存镜像为指定⽂件
docker save -o mynginx.tar mynginx:v1.0
 
删除多个镜像
docker rmi bde7d154a67f 94543a6c1aef e784f4560448
```
