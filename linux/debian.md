# 基于 debian 12 OS

## 添加用户
因为初始默认使用的是 **root** 用户，**root** 用户权限极大，一旦 **root** 密码泄露，将导致服务器被入侵。所以建议登录成功后及时 **添加用户**、**修改 SSH 端口**、**关闭 root 登录**、或使用 **SSH Key** 登录，以增强服务器的安全性。
1. 创建用户:
    ```bash
    adduser username
    ```
    > 说明: 添加用户名为 **username** 的用户。

2. 添加用户权限:
    ```bash
    usermod -aG sudo username
    ```
    > 说明: 将用户添加到 **sudo** 组，使得用户可以执行 **root** 权限下的命令。
    > 1. **-a**: 添加用户到指定的组。
    > 2. **-G**: 指定用户要加入的组。
    > 3. **sudo**: 指定要添加的用户组。

3. 切换用户:
    ```bash
    su username
    ```
    > 意外情况，使用 **sudo xxxx** 时出现 **-bash: sudo command not found**
    > 1. 检查 **sudo** 是否安装: **which sudo**
    > 2. 如果没有安装, 则安装: **apt install sudo**

## 修改 ssh 登录:
1. 修改 ssh 配置:
    ```bash
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
    > 说明:
    > 1. **Port**: 指定 ssh 端口。
    > 2. **PermitRootLogin**: 指定是否允许 root 登录。
    > 3. **PasswordAuthentication**: 指定是否允许密码登录。
    > 4. **PubkeyAuthentication**: 指定是否允许 ssh key 登录。
    > 5. **AllowUsers**: 指定允许登录的用户。

2. 添加 ssh key:
    ```bash
    mkdir ~/.ssh
    echo "<public_key>" >> ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/authorized_keys
    ```
    > 说明:
    > 1. **mkdir ~/.ssh**: 创建 .ssh 目录。
    > 2. **<public_key>**: 指定 ssh key 的公钥。
    > 3. **>>**: 将内容追加写入 authorized_keys 文件。
    > 3. **chmod 600**: 指定 ssh key 的权限。

3. 重启 ssh 服务:
    ```bash
    sudo service sshd restart
    ```

4. 测试 ssh 登录:
    ```bash
    ssh -p 22 -i ~/.ssh/id_rsa username@ip
    ```
    > 说明:
    > 1. **-p**: 指定 ssh 端口。默认是 22 端口，可以省略此参数。
    > 2. **-i**: 指定 ssh key 的路径。
    > 3. **username**: 指定用户名。
    > 4. **ip**: 指定服务器的 ip。

## 开启 swap
swap 是 linux 中的一个虚拟内存，如果内存不足，建议在服务器上开启 swap，以解决内存不足的问题。
1. 查看 swap 状态:
    ```bash
    free -m
    ```
    > 说明:
    > 1. **-m**: 以 MB 为单位显示。
    > 2. 如果 **swap** 为 0, 则需要开启 swap。
2. 开启 swap:
    ```bash
    sudo fallocate -l 4G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile
    echo "/swapfile none swap sw 0 0" | sudo tee -a /etc/fstab
    free -m
    ```
    > 说明:
    > 1. **fallocate**: 创建一个指定大小的文件。建议swap大小为内存的 2 倍。
    > 2. **chmod**: 修改文件权限。
    > 3. **mkswap**: 格式化 swap 文件。
    > 4. **swapon**: 启用 swap 文件。
    > 5. **echo "/swapfile none swap sw 0 0" | sudo tee -a /etc/fstab**: 将 swap 文件添加到 fstab 文件中，使得 swap 文件在重启后自动启用。
