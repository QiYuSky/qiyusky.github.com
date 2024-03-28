# Fail2ban

1. 安装

        apt-get install fail2ban
1. 启用

        systemctl enable fail2ban.service
1. 查看运行状态

        systemctl status fail2ban.service
1. 配置参数

        vim /etc/fail2ban/jail.d/jail.local
    >- #规则
        [sshd]
    >- #是否启用
        enabled = true
    >- #行为规则模式 (对应.../action.d/ufw.conf)
        banaction = ufw
    >- #监听端口 (ssh端口|22)
        port = ssh
    >- #过滤规则
        filter = sshd
    >- #日志路径
        logpath = /var/log/auth.log
    >- #失败次数
        maxretry = 3
    >- #禁止时间 1h 1m 60(默认 s)
        bantime = 1h 
1. 重启服务

        systemctl restart fail2ban.service
1. 使用
    - 禁止 ip
        > fail2ban-client set sshd unbanip 192.168.1.111
    - 解除禁止 ip
        > fail2ban-client set sshd unbanip 192.168.1.111
    
1. 查看启用的规则状态

        fail2ban-client status
1. 查看具体规则状态

        fail2ban-client status sshd