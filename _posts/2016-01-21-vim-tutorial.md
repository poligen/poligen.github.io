---
layout: post
title: "開始用vim 寫作業"
date: 2016-01-21 16:55 +0800
categories: programming
tag: vim
---
# 想法
中文的話有[鳥哥](http://linux.vbird.org/linux_basic/0310vi.php)、[大家來學vim](http://www.study-area.org/tips/vim/), 雖然都是上了年紀了。但都是很好的資源喔。
開始想要學vim 是因為：   
1. 用atom editor:   
雖然是免費的，但是一但開太多分頁，使用的系統會變很慢。CPU被吃很多資源。我在github上的issue看過很多解法（例如重灌atom、不用太多的plugins）也或許我是用老電腦了（五年前的了……）但常常變慢，覺得不是很開心的使用者經驗。    
<!--more-->

2. 其實可以用sublime text 2？   
外觀和atom差不多，套件多一點、也比較不會吃資源、易上手，的確是atom好的替用品，不過要付70元美金，不然會一直跳廣告。最近看教學影片有不少人用vim，想說這麼古老的editor，怎麼還會有人再用呢？常常會有這樣的疑惑。剛好友人也說最近他也曾sublime跳到vim了，順手推了我一把。開始研究之後，發現真的有不少有趣的東西。  

開頭可以看這個vim-walking-without-crutches[投影片](https://speakerdeck.com/nelstrom/vim-precision-editing-at-the-speed-of-thought)/[影片](http://vimeo.com/53144573)（建議看影片比較詳細），看完對於vim背後的哲理就可以比較理解.  
裡頭總結：   
  1. 用vim 就是modal interface   
  2. 用vim 就像是玩street fighter, 可以接連續技！！   


![連續技](https://farm5.staticflickr.com/4079/4824433241_53bd9ce2b8_z.jpg)

# 安裝
有一些常用的plugin，還有安裝方法，我不想一個個去找，網路也都有推薦。直接使用這個：[janus](https://github.com/carlhuda/janus)

## 無痛起步教學
安裝好後，可以在terminal直接輸入'vimtutor'，可以得到入門票卷一張。
再來，可以來[vim adventure](http://vim-adventures.com/)來練習。免費版有三章，練完前三章，就可以知道怎麼移動vim裡頭的光標位置了。
[快速教學](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/#navigation)

## 教學影片
我個人是把[Derek Wyatt's](http://derekwyatt.org/vim/tutorials/)影片看到intermediate的部分，一邊看、一邊開著簡單的文檔一起操作。
再搭配好用的cheatsheet [A](http://blog.vgod.tw/2009/12/08/vim-cheat-sheet-for-programmers/), [B](http://www.viemu.com/a_vi_vim_graphical_cheat_sheet_tutorial.html), 就可以對初步的使用有很好的理解。

## 好用的書
如果還要好好的熟練，我現在是用這本practice vim, 每天練個幾個tips，來熟練vim。學vim 的過程像是學輸入法、學一種工具而已。一種工具是一種信仰啦。我覺得用vim 的這幾天，雖然有點慢、偶爾要想一下，但不用再用滑鼠移來移去，感覺不錯！

# others

## janus install

```
setting in /.janus   
~/.vimrc.before for customization   
~/.vimrc.after    
```
因為janus都設定好了，如果要再自行設定，要自己用個before/after檔，差異是內建plugin讀入前後的設定，怕麻煩通通用after 就好。如果要調整default的設定或是之後要再裝plugin（自己裝的要放在/.janus），設定在/.vimrc.after。就可以了。

## other plugins
除了janus的vim 套件外，還有一些我也建議要安裝的：   
- NERDTree-tabs   
- vim indent guides    

## Theme
janus有內建很多theme，有興趣可以一個個試試，不過我個人後來都用這個有內建，不需再安裝
[vim-colors-solarized](https://github.com/altercation/vim-colors-solarized)
(這個solarized在很多editor都有出現。)
只要改一下，就可以啟動時預設

in .vimrc.after:
``` bash
syntax enable
set background=dark
colorscheme solarized


```

## 字型方面
我主要都是用如上篇zsh裡頭的字型，如果vim在啟用時要設定字型，可以google 一下相關設定。
在/.vimrc.before:

``` bash
set guifont=Inconsolata\ for\ Powerline\ Medium\ 13
```

## 圖型介面
mac有macvim
linux/window有gvim, 但gvim還是在linux下才可發輝功能啊！

## terminal setting
terminal 的話，在ubuntu/linux mint下改用terminator
```
sudo apt-get update
sudo apt-get install terminator
```

## cheatsheet
[beautiful](http://vimcheatsheet.com/), but cost 10USD
這個cheatsheet可以買pdf之後再去輸出海報，除了當裝飾之外，有時忘了快速鍵，抬頭一看就有啦！
## others
因為有在insert mode 想要移動[cursor](http://stackoverflow.com/questions/1737163/traversing-text-in-insert-mode)的功能。查了一下，以下是相關的設定：  
在.vimrc.before裡:
```
"" provide hjkl movements in Insert mode via the <Alt> modifier key
inoremap <A-h> <C-o>h
inoremap <A-j> <C-o>j
inoremap <A-k> <C-o>k
inoremap <A-l> <C-o>l

```
這樣在vim 的insert mode裡就可以用alt+hjkl移動了，不用在按方向鍵或ESC回normal mode
