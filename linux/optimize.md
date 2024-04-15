# 优化服务器

****

## 基本优化
1. 替换源:
因为默认使用的是 **Debian** 源，在国内访问速度会很慢，所以可以用 **阿里云**、**腾讯云**、**华为云** 等源来替换，这样速度会更快。
    ```bash
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
    sudo vim /etc/apt/sources.list
    # 替换源
    deb http://mirrors.aliyun.com/debian/ bookworm main non-free non-free-firmware contrib
    deb-src http://mirrors.aliyun.com/debian/ bookworm main non-free non-free-firmware contrib
    deb http://mirrors.aliyun.com/debian/ bookworm-updates main non-free non-free-firmware contrib
    deb-src http://mirrors.aliyun.com/debian/ bookworm-updates main non-free non-free-firmware contrib
    deb http://mirrors.aliyun.com/debian/ bookworm-backports main non-free non-free-firmware contrib
    deb-src http://mirrors.aliyun.com/debian/ bookworm-backports main non-free non-free-firmware contrib
    deb http://mirrors.aliyun.com/debian-security/ bookworm-security main non-free contrib
    deb-src http://mirrors.aliyun.com/debian-security/ bookworm-security main non-free contrib
    # 保存退出
    :wq
    # 应用源
    sudo apt-get update
    sudo apt-get upgrade -y
    ```
    > 说明: 替换源为 **Aliyun** 的源。

2. 安装常用软件:
    ```bash
    sudo apt-get update
    sudo apt-get install -y vim git curl wget htop net-tools
    ```
    > 说明:
    > 1. **vim**: 编辑器。
    > 2. **git**: 版本控制。
    > 3. **curl**: 网络工具。
    > 4. **wget**: 网络工具。
    > 5. **htop**: 进程管理。
    > 6. **net-tools**: 网络工具。

3. 设置时区:
    ```bash
    sudo timedatectl set-timezone Asia/Shanghai
    ```
    > 说明: 设置时区为 **Asia/Shanghai**。
    > 1. **timedatectl**: 查看当前时区。
    > 2. **set-timezone**: 设置时区。
    > 3. **Asia/Shanghai**: 时区为 **Asia/Shanghai**。

4. 设置 hostname:
    ```bash
    sudo hostnamectl set-hostname server
    ```
    > 说明: 设置 hostname 为 **server**。
    > 1. **hostnamectl**: 查看当前 hostname。
    > 2. **set-hostname**: 设置 hostname。
    > 3. **server**: hostname 为 **server**。

