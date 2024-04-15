# PODMAN

podman 是一个容器管理工具，可以很方便的管理容器。它与 docker 相比，比 docker 更轻量级，而且支持更多的功能。比如，docker 需要 root权限，而 podman 不需要。

## 准备环境

```bash
# 安装依赖
sudo apt-get install podman
# 启动服务
sudo systemctl start podman.socket
sudo systemctl enable podman.socket
```

## 使用 docker 镜像仓库

```bash
sudo vim /etc/containers/registries.conf
# 添加以下内容
unqualified-search-registries = ["docker.io"]
```

## 基本命令

```bash
# 搜索镜像
podman search <image_name>
# 拉取镜像
podman pull <image_name>
# 列出所有镜像
podman images
# 删除镜像
podman rmi <image_id>
# 列出所有容器
podman ps -a
# 启动容器
podman start <container_id>
# 停止容器
podman stop <container_id>
# 删除容器
podman rm <container_id>
# 查看容器日志
podman logs <container_id>
# 进入容器
podman exec -it <container_id> bash
```

## 运行容器

```bash
podman run -itd --name <container_name> -p <host_port>:<container_port> -v <host_path>:<container_path> <image_name>
# -i 表示交互模式
# -t 表示分配一个伪终端
# -d 表示后台运行
# --name <container_name> 表示给容器起一个名字
# -p <host_port>:<container_port> 表示将容器的 <container_port> 端口映射到宿主机的 <host_port> 端口
# -v <host_path>:<container_path> 表示将宿主机的 <host_path> 路径映射到容器的 <container_path> 路径
# 示例
podman run -d --name mysql -p 3306:3306 -v /home/<user>/mysql:/var/lib/mysql mysql:8.0.32
```

## 文件路径

1. 配置文件路径
    1. root 用户的路径是 **/etc/containers/containers.conf**
    2. 普通 用户的路径是 **～/.config/containers/containers.conf**
2. 容器配置文件保存
    1. root 用户的路径是 **/var/lib/containers/storage/overlay**
    2. 普通用户的路径是 **～/.local/share/containers/storage/overlay**

## 扩展知识

1. 因为 podman 是无根的，所以用户退出后，启动的容器会自动停止。可以使用 **sudo loginctl enable-linger** 启用linger模式，使得在系统关闭时，当前用户的服务能够继续运行
2. 容器内需要访问宿主机时，可以使用 **host.containers.internal** 替代宿主机 ip
