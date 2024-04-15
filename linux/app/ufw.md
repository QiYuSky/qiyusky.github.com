# UFW

ufw 是一个基于iptables的防火墙管理工具。它提供了一个易于理解和使用的命令行界面，使用户能够快速设置防火墙规则以控制进出系统的网络流量。

## 准备环境

```bash
# 安装
sudo apt-get install ufw
# 添加别名
echo "alias ufw='/usr/sbin/ufw'" >> ~/.zshrc
source ~/.zshrc
# 启用防火墙
ufw enable
```

## 基本语法

```bash
# 关闭防火墙
ufw disable
# 重载规则策略
ufw reload
# 重置规则策略
ufw reset
# 查看规则状态
ufw status verbose
```

## 配置

1. 使用默认策略

    ```bash
    # 设置默认策略
    ufw default allow|deny [incoming|outgoing]
    # allow 允许
    # deny 拒绝
    # incoming 表示接收
    # outgoing 表示发送
    # 样例
    ufw default allow incoming
    ```

2. 按照服务进行配置

    ```bash
    # 允许指定服务访问
    ufw allow|deny [service]
    # 样例
    ufw allow ssh
    ```

3. 按照端口进行配置

    ```bash
    # 允许指定端口访问
    ufw allow|deny [port]/[protocol]
    # 样例
    ufw allow 22/tcp
    ```

4. 按照 ip 进行配置

    ```bash
    # 允许指定ip访问
    ufw allow|deny [ip]/[mask]
    # 样例
    ufw allow from 192.168.0.1
    ufw allow from 192.168.0.1/24
    ufw allow from 192.168.0.1 to any port 80
    ufw allow from any to any port 80
    ```

## 删除规则

1. 删除指定规则

    ```bash
    # 删除规则
    ufw delete [rule]
    # 样例
    ufw delete allow ssh
    ```

2. 根据规则序号删除

    ```bash
    # 查看规则序号
    ufw status numbered
    # 删除
    ufw delete [number]
    # 样例
    ufw delete 1
    ```

3. 删除所有规则

    ```bash
    ufw reset
    ```

## 转发策略

1. 设置允许转发的包

    ```bash
    # 打开配置文件
    sudo vim /etc/default/ufw

    # 允许转发
    DEFAULT_FORWARD_POLICY="ACCEPT"

    # 保存退出
    :wq
    ```

2. 启用内核转发

    ```bash
    # 修改配置
    sudo sysctl -w net.ipv4.ip_forward=1
    ```

    or

    ```bash
    sudo vim /etc/ufw/sysctl.conf

    # 修改配置
    net/ipv4/ip_forward=1

    # 保存退出
    :wq
    ```

3. 重载配置

    ```bash
    ufw reload
    ```
