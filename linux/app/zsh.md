# zsh
1. 安装前准备组件依赖

        sudo apt-get install zsh curl wget git vim -y
1. 下载 zsh

        sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
        or
        sh -c "$(wget -O- https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"
1. 设置 zsh 为默认环境并切换到 zsh

        chsh -s $(which zsh)
        exec zsh
1.  下载 p10k 主题

        git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
1. 下载 语法高亮 插件

        git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
1. 下载 命令行自动补全 插件

        git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
1. 配置主题和插件扩展

        vim ~/.zshrc
        
    >- ZSH_THEME="powerlevel10k/powerlevel10k"
    >- plugins=(
    zsh-autosuggestions
    zsh-syntax-highlighting
    )
1. 应用修改

        source ~/.zsh