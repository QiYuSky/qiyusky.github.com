# 基于 debian 12 OS

## 添加用户

因为初始默认使用的是 **root** 用户，**root** 用户权限极大，一旦 **root** 密码泄露，将导致服务器被入侵。所以建议登录成功后及时 **添加用户**、**修改 SSH 端口**、**关闭 root 登录**、或使用 **SSH Key** 登录，以增强服务器的安全性。

1. 创建用户:

    ```bash
    # 创建用户名为 username 的用户
    adduser username
    ```

2. 添加用户权限:

    ```bash
    # 添加用户 username 到 sudo 组
    usermod -aG sudo username
    ```

3. 切换用户:

    ```bash
    su username
    ```

    * 如果出现 意外情况，使用 **sudo xxxx** 时出现 **-bash: sudo command not found**
    > 1. 检查 **sudo** 是否安装: **which sudo**
    > 2. 如果没有安装, 则安装: **apt install sudo**

## 修改 ssh 登录

1. 修改 ssh 配置:

    ```bash
    # 编辑配置
    sudo vim /etc/ssh/sshd_config

    # 修改配置
    Port 22
    PermitRootLogin no
    PasswordAuthentication no
    PubkeyAuthentication yes
    AllowUsers username

    # 保存退出
    :wq
    ```

2. 添加 ssh key:

    ```bash
    # 创建 .ssh 目录
    mkdir ~/.ssh

    # 将 public_key 添加到 authorized_keys 中。 public_key: 指定 ssh key 的公钥。
    echo "<public_key>" >> ~/.ssh/authorized_keys

    # 修改权限
    chmod 600 ~/.ssh/authorized_keys
    ```

3. 重启 ssh 服务:

    ```bash
    sudo service sshd restart
    ```

4. 测试 ssh 登录:

    ```bash
    # ssh 登录
    ssh -p 22 -i ~/.ssh/id_rsa username@ip

    # 说明:
    # -p : 指定 ssh 端口。默认是 22 端口，可以省略此参数。
    # -i : 指定 ssh key 的路径。
    # username@ip: 指定用户名和服务器的 ip。
    ```

## 开启 swap

swap 是 linux 中的一个虚拟内存，如果内存不足，建议在服务器上开启 swap，以解决内存不足的问题。

1. 查看 swap 状态:

    ```bash
    # 查看 swap 状态，-m: 以 MB 为单位显示。
    free -m

    # 如果 swap 显示 0, 则需要开启 swap。
    ```

2. 开启 swap:

    ```bash
    # 创建一个指定大小的文件。建议swap大小为内存的 2 倍。
    sudo fallocate -l 4G /swapfile

    # 修改文件权限
    sudo chmod 600 /swapfile

    # 格式化 swap 文件
    sudo mkswap /swapfile

    # 启用 swap
    sudo swapon /swapfile

    # 将 swap 文件添加到 fstab 文件中，使得 swap 文件在重启后自动启用。
    echo "/swapfile none swap sw 0 0" | sudo tee -a /etc/fstab

    # 查看 swap 状态
    free -m
    ```
