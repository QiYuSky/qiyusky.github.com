# Fail2ban

1. 安装

        apt-get install fail2ban
1. 启用

        systemctl enable fail2ban.service
1. 查看运行状态

        systemctl status fail2ban.service
1. 配置参数

        vim /etc/fail2ban/jail.d/jail.local

    >- #规则 <br> [sshd]
    >- #是否启用 <br> enabled = true
    >- #行为规则模式 (对应.../action.d/ufw.conf) <br> banaction = ufw
    >- #监听端口 (ssh端口|22) <br> port = ssh
    >- #过滤规则 <br> filter = sshd
    >- #日志路径 <br> logpath = /var/log/auth.log
    >- #失败次数 <br> maxretry = 3
    >- #禁止时间 1h 1m 60(默认 s) <br> bantime = 1h 
1. 重启服务

        systemctl restart fail2ban.service
1. 使用
    1. 禁止 ip
        > fail2ban-client set sshd unbanip 192.168.1.111
    1. 解除禁止 ip
        > fail2ban-client set sshd unbanip 192.168.1.111
    1. 查看启用的规则状态
        > fail2ban-client status
    1. 查看具体规则状态
        > fail2ban-client status sshd