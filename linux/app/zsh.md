# ZSH
zsh 是一个功能强大的 shell，可以用来替代默认 bash。

## 使用 **zsh** 作为默认终端:
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
chsh -s $(which zsh)
```
> 说明: 
> 1. 使用远程脚本来安装 **zsh**。
> 2. **chsh -s $(which zsh)**: 设置 **zsh** 为默认终端。

## 使用 **zsh** 插件:
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
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
> 说明: 
> 1. **zsh-autosuggestions**: 自动提示。
> 2. **zsh-syntax-highlighting**: 语法高亮。
> 3. **git**: 版本控制。

## 使用 **zsh** 主题:
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
> 说明: 应用后会自动执行一遍 **p10k configure** 命令，会弹出一个窗口，根据里面的选项进行设置自定义。