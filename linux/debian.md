# 基于 debian 12 OS

[用户管理](#section1) | [替换镜像包源](#section2) | [SSH登录](#section3)

## 用户管理 <a id="section1"></a>
1. 创建新用户
        
        adduser <userName>
1. 添加用户到超级管理员组

        usermod -aG sudo <userName>
1. 切换用户

        su -l <userName>
1. (意外) 修复 -bash: sudo command not found 

        find /etc/sudoers.d
        apt-get install sudo

***
## 替换镜像包源 <a id="section2"></a>
1. 备份源文件

        sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
1. 切换到 root 用户

        sudo su -
1. 写入阿里云镜像地址

        cat /etc/apt/sources.list <<-EOF
        deb https://mirrors.aliyun.com/debian/ bookworm main non-free non-free-firmware contrib
        deb-src https://mirrors.aliyun.com/debian/ bookworm main non-free non-free-firmware contrib
        deb https://mirrors.aliyun.com/debian-security/ bookworm-security main
        deb-src https://mirrors.aliyun.com/debian-security/ bookworm-security main
        deb https://mirrors.aliyun.com/debian/ bookworm-updates main non-free non-free-firmware contrib
        deb-src https://mirrors.aliyun.com/debian/ bookworm-updates main non-free non-free-firmware contrib
        deb https://mirrors.aliyun.com/debian/ bookworm-backports main non-free non-free-firmware contrib
        deb-src https://mirrors.aliyun.com/debian/ bookworm-backports main non-free non-free-firmware contrib
1. 输入 `EOF` 结束命令输入
1. 使用 `ctrl + d` 退出 root 用户
1. 刷新并使用源地址

        sudo apt-get update & sudo apt-get upgrade -y

***
## vim
1. 安装

        sudo apt-get install vim
2. 编辑用户配置

        ~/.vimrc   
        or
        /etc/vim/vimrc
        
    >- #设置行号
        set nu          
    >- #设置tab为4个空格
        set ts=4        
    >- #设置自动换行
        set autoindent  
    >- #设置高亮字符 ------ 在命令行 :noh 取消高亮
        set hlsearch    
    >-  #设置vim打开文件，光标保持最下面有5行，最上面也是一样的
        set scrolloff=5
        
***
## ssh 登录 <a id="section3"></a>
1. 编辑 ssh 配置

        sudo vim /etc/ssh/sshd_config
        
    >- #监听端口  默认22
        Port xxx
    >- #是否允许管理员直接登录
        PermitRootLogin yes
    >- #最大认证尝试次数
        MaxAuthTries 6
    >- #允许的最大会话数
        MaxSessions 10
    >- #是否允许支持基于口令的认证
        PasswordAuthentication yes
    >- #是否允许公钥认证
        PubkeyAuthentication yes
1. 重启 ssh 以应用修改

        sudo systemctl restart ssh
1. 创建当前用户的 ssh 配置

        cd ~
        mkdir .ssh
1. 写入公钥到文件中

        echo "<public_key>" >.ssh/authorized_keys

1. 修改公钥文件权限

        chmod 600 .ssh/authorized_keys
1. 检查当前服务器地址

        ip address
3. ssh 登录演示
    >- ssh user@1.1.1.1    
    >- ssh -p 22 user@1.1.1.1
    >- ssh -i ~/ssh_file user@1.1.1.1