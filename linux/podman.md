# Podman

1. 安装

        sudo apt-get install podman
1. 启动服务

        sudo systemctl start podman
1. 设置为开机启动

        sudo systemctl enable podman
1. 基础命令

    参数|备注|命令|额外参数
    :--:|:--:|:--|:--
    search  |搜索镜像   |podman search nginx    
    pull    |拉取镜像,默认最新版本   |podman pull nginx  |:xx 指定拉取版本
    images  |查看本地所有镜像(当前用户) |podman images
    ps      |查看运行中容器 |podman ps  |-a 显示所有容器
    run     |运行容器   |podman run nginx   |-d 后台运行<br> --name 指定名字<br>
    rm      |删除容器   |podman rm nginx    |-f 删除运行中的容器
    rmi     |删除镜像   |podman rmi nginx
    logs    |查看容器日志   |podman logs nginx  |
    exec    |进入容器内部   |podman exec -it nginx /bin/bash    |-it 退出容器后不会关闭容器

***
1. 修复拉取镜像时错误

        sudo vim /etc/containers/registries.conf
    >-  unqualified-search-registries = ["docker.io"]
1. 配置文件路径
    - 根配置
        > /usr/share/containers/containers.conf
    - 根用户 会覆盖根配置
        > /etc/containers/containers.conf
    - 普通用户 会覆盖根配置
        > $HOME/.config/containers/containers.conf
1. 仓库配置文件
    - 根配置
        > /etc/containers/registries.conf
        > /etc/containers/registries.d/*
    - 用户配置 会覆盖根配置
        > HOME/.config/containers/registries.conf