## 用户管理

```bash
> useradd <username>	% add user
> passwd <username>	% change password for user: username
```

## Shell

### Zsh

#### sudo

**Install**

```Bash
$ vim ~/.zshrc
# add "sudo" to the plugins list
```



#### autojump

**Install**

```Bash
$ sudo apt install python
$ git clone git://github.com/wting/autojump.git
$ cd autojump
$ ./install.py
$ vim ~/.zshrc
# add "autojump" to the plugin 
```

#### zsh-syntax-highlighting

**Install**

```Bash
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

$ vim ~/.zshrc
# add to the plugins

$ source ~/.zshrc
```

#### zsh-autosuggestions

**Install**

```Bash
$ git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions

$ vim ~/.zshrc
$ source ~/.zshrc
```



## SSH

```bash
# remote log in without break out from time to time
> ssh -o serveraliveinterval=60 username@hostIP 
```



## 语言

打开txt，出现中文乱码的情况
参考：[Ubuntu 18.04 中TXT中文显示乱码的解决办法](https://www.jrjxdiy.com/linux/ubuntu-18-04-the-solution-of-displaying-messy-code-in-chinese-for-txt-file.html)

```bash
sudo apt install dconf-tools

dconf-editor
```
打开 `org/gnome/gedit/preferences/encodings/candidate-encodings` 

* 关闭`Using default values`

* `custom values` 添加`['UTF-8′,'GB18030′,'GB2312′,'GBK’,'BIG5′,'CURRENT','UTF-16′]`
