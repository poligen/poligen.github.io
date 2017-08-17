---
layout: post
title: "在ubuntu/linux-mint下設定zsh/oh-my-zsh"
date: 2016-01-15 21:25 +0800
categories: programming
tag: [zsh, oh-my-zsh]
---

最近被友人推坑，zsh好用的功能。使得原本shell的功能得以加強啊。不過網路上大多是mac版本的設定。
所以還是把linux的設定查一查之後，寫好文章以後給自己看用的。
<!--more-->
1. 先要安裝zsh:  
`sudo apt-get install git zsh`
2. 再來改預設成zsh:  
`chsh -s /bin/zsh`
3. 安裝[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh):  
`sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"`
4. 安裝特殊的字型: [power-patched font](https://github.com/powerline/fonts)  
  下載zip後，在terminal 執行`./install.sh`即可
5. 下載[dir-colors](https://github.com/seebi/dircolors-solarized)讓directory有不同顏色:
  - 在使用者目錄下新建一個資料夾"/.dircolors"
  - 把`dircolors.ansi-dark`下載下來後，放入此資料夾
  - 在`/.zshrc`中，最後一行加入`eval `dircolors ~/.dircolors'

6. 下載 [gnome-terminal-colors-solarized ](https://github.com/Anthony25/gnome-terminal-colors-solarized):
  - 下載整個資料夾，解壓縮
  - `sudo apt-get install dconf-cli`
  - 之後跑`./set_dark.sh`
7. 設定gnome terminal的字型（不然git 插件的特殊字體會跑走)
  - 選擇Inconsolata 或menlo都可
  - 選擇"可使用粗體字"

8. plugins 的部分可以用:  
`ls ~/.oh-my-zsh/plugins`來查看

9. 若是使用rvm 來安裝ruby/rails
  - 可能要去`.bash_profile`, 複製`[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"`，到`.zshrc`，且放到第一行。  
  - 另外要把`export PATH=/path/to/something`, 改成`export PATH="$PATH:/path/to/something"`
    否則會有錯誤訊息。
  source: [RVM not working for me](https://gist.github.com/jacqueline-homan/8b2035ada0d6a2bded15), [stackoverflow](http://stackoverflow.com/questions/27784961/received-warning-message-path-set-to-rvm-after-updating-ruby-version-using-rvm)

10. 設定/.zshrc, 以下這幾個部分是新增或是有修改的:  

``` bash
  # Path to your oh-my-zsh installation.  
  export ZSH=/home/username/.oh-my-zsh
  ZSH_THEME="agnoster"
  ENABLE_CORRECTION="true"
  COMPLETION_WAITING_DOTS="true"
  plugins=(git ruby rvm)
  # Used by agnoster theme
  DEFAULT_USER="username"
  # Auto load solarized dark color theme
  eval `dircolors ~/.dir_colors`

```

就可以開開心心使用zsh了。
接下來就是好好學vim的時候了。
