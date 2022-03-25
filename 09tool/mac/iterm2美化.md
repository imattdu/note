



![](https://raw.githubusercontent.com/imattdu/img/main/img/202202130255589.png)







## 安装



### 安装iterm2

安装iterm2，[官网](https://iterm2.com/)下载安装即可 

官网下载iterm2





### 配置代理

git、github（可参考git文章）、终端配置代理







终端配置代理

```sh
cd ~
vim ./.zshrc
```



```sh
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890
```



配置文件生效

```sh
source ./.zshrc
```





### 安装oh-my-zsh





```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```







### 安装 p10k

```sh
cd ./.oh-my-zsh/themes
git clone  https://github.com/romkatv/powerlevel10k.git
```





```sh
vi ~/.zshrc 
```



```sh
ZSH_THEME="powerlevel10k/powerlevel10k"
```

### 安装插件



#### 语法高亮、 自动补全



```sh
# 语法高亮
cd ~/.oh-my-zsh/custom/plugins/
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git


# 自动补全

cd ~/.oh-my-zsh/custom/plugins/
git clone https://github.com/zsh-users/zsh-autosuggestions
```



退出iterm2

```sh
exit
```





```sh
vim ./.zscrc
```





```sh
plugins=(
     git
     zsh-syntax-highlighting
     zsh-autosuggestions
)


HIST_STAMPS="yyyy-mm-dd"


# 文末添加
source ~/.zshrc.pre-oh-my-zsh
```





#### 安装autojump





```sh
brew install autojump

```



打开 `~/.zshrc` 加一行代码：

```sh
[[ -s $(brew --prefix)/etc/profile.d/autojump.sh ]] && . $(brew --prefix)/etc/profile.d/autojump.sh
```



使用

```sh
j test
```



#### 开启sublime插件







```sh
vim ~/.zshrc
```



plugins添加sublime

```sh
plugins=(
     git
     git-open
     sublime
     zsh-syntax-highlighting
     zsh-autosuggestions
)
```





```sh
alias subl="/Users/matt/Downloads/Sublime\ Text.app/Contents/SharedSupport/bin/subl"
```







#### git-open



```sh
git clone https://github.com/paulirish/git-open.git
```





```sh
plugins=(其他的插件 git-open)
```



```sh
alias go="git-open"
```







#### rm 

trash代替rm,可以将删除的文件放入回收站



```sh
npm install --global trash-cli
```





```sh
alias rm="trash"
```





## 配置



### p10k 配置





**前提：配置好代理**

```sh
p10k configure
```









问题大致如下：

1. 这个符号看起来像钻石（旋转的正方形）吗？
2. 这个符号看起来像锁吗？
3. 这个符号看起来像 Debian logo 吗？
4. 这些图标都交叉分布在 X 之间吗？
5. 风格
6. 编码
7. 是否显示时间
8. 目录层级分隔符
9. 头部（左边）
10. 尾部（右边）
11. 是否换行
12. 左边和右边是否有连接线
13. 命令行和提示是否连接
14. 两行命令之间分布稀疏还是松散
15. 是否需要图标









### 字体

第一步会下载字体，如果失败记得配置代理





https://github.com/ryanoasis/nerd-fonts/releases

![](https://raw.githubusercontent.com/imattdu/img/main/img/202202130241326.png)









![](https://raw.githubusercontent.com/imattdu/img/main/img/202202130242317.png)









### color







https://github.com/QuentinWatt/dark-flat-iterm-colors









![](http://raw.githubusercontent.com/imattdu/img/main/img/202202142221356.png)











https://gist.github.com/kevin-smets/8568070



0, 40, 51

















