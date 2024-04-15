# FAIL2BAN
fail2ban 是 linux 下一个很常用的安全工具，通过监控系统日志文件，识别出重复且无效的登录尝试（如SSH、FTP、HTTP等服务），然后根据预定义的规则自动将发起这些攻击的 IP 地址添加到防火墙规则中，暂时禁止它们的访问。

## 准备环境
```bash
# 安装
sudo apt-get install fail2ban
# 启动服务
sudo systemctl start fail2ban.service
sudo systemctl enable fail2ban.service
# 查看运行状态
sudo systemctl status fail2ban.service
```

## 配置 ssh 服务 
```bash
# 编辑日志规则过滤器(可选)
sudo vim /etc/fail2ban/filter.d/sshd.conf

# 编辑服务规则
sudo vim /etc/fail2ban/jail.d/jail.local

#规则
[sshd]
enabled = true
banaction = ufw
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
findtime = 30m
bantime = 10h
ignoreip = 127.0.0.1

# 保存退出
:wq

# 重启服务
sudo systemctl restart fail2ban.service
```
> 说明：
> **sshd** 是 sshd 服务的配置文件
> **enabled** 是否启用该规则
> **banaction** 指定封禁方式，可选 ufw、iptables、reject
> **port** 监听的服务端口，可以直接指定端口号
> **filter** 关联的过滤器，定义如何解析日志文件并识别攻击模式
> **logpath** 监控的日志文件路径
> **maxretry** 在指定时间内连续失败登录的最大次数，超过这个次数则触发封禁
> **findtime** 检测时间，默认单位是 s
> **bantime** 禁止访问时间，可以配置为 1m 1h 1d，默认单位是 s
> **ignoreip** 要忽略的 IP 地址列表，这些地址不会被监控

## 其他使用
```bash
# 查看所有规则状态
fail2ban-client status 
# 查看具体规则状态
fail2ban-client status sshd
# 禁止 ip
fail2ban-client set sshd banip 192.168.1.111
# 解除禁止
fail2ban-client set sshd unbanip 192.168.1.111
```