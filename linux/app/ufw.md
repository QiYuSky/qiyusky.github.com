# UFW 

1. 安装

        apt-get install ufw
1. 配置环境

        echo "alias ufw='/usr/sbin/ufw'" >> ~/.zshrc
1. 基础
    - 开启防火墙
        > ufw enable
    - 关闭防火墙
        > ufw disable
    - 重载规则策略
        > ufw reload
    - 重置规则策略
        > ufw reset
    - 查看规则状态
        > ufw status verbose
    
1. 规则策略
    - 启用默认允许通过策略
        > ufw default allow
    - 启用默认拒绝通过策略
        > ufw default deny
    - 允许规则
        >- ufw allow ssh
        >- ufw allow 22
        >- ufw allow 80/tcp
        >- ufw allow 9000:9002/tcp
        >- ufw allow from 192.168.0.1
        >- ufw allow from any
        >- ufw allow from 192.168.0.1 to any port 80
        >- ufw allow from any to any port 80
    - 拒绝规则
        >- ufw deny ssh
    - 删除规则
        >- ufw delete allow 22
        >- ufw delete deny 23

1. 转发策略
    1. 允许接收转发的包
        
            vim /etc/default/ufw
                
        > DEFAULT_FORWARD_POLICY="ACCEPT"
    1. 启用内核转发

            vim /etc/ufw/sysctl.conf
        
        > net/ipv4/ip_forward=1
    1. 重载配置

            ufw reload