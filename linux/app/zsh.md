# ZSH

zsh 是一个功能强大的 shell，可以用来替代默认 bash。

## 使用 **zsh** 作为默认终端

```bash
# 安装 oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# 设置 zsh 为默认终端
chsh -s $(which zsh)
```

## 使用 **zsh** 插件

```bash
# 下载自动提示插件
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# 下载语法高亮插件
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
vim ~/.zshrc
# 添加插件
plugins=(
zsh-autosuggestions
zsh-syntax-highlighting
git
)
# 保存退出
:wq
source ~/.zshrc
```

## 使用 **zsh** 主题

```bash
git clone https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
vim ~/.zshrc
# 修改主题
ZSH_THEME="powerlevel10k/powerlevel10k"
# 保存退出
:wq
source ~/.zshrc
p10k configure
```
