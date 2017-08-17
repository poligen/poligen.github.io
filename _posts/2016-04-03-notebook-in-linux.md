---
layout: post
title: "安裝完ubuntu/linux mint在筆電下的快速設定"
date:   2016-04-03 21:30:19 +0800
categories: programming
tag: [linux, tlp, ubuntu]
---
### linux電源管理神器tlp
筆電因為很少帶來帶去，對於電池的電量不是很在乎。也不太會用到太多的電量。不過因為最近開始有需要筆電帶來帶去的需要，  
開始有電池管理的需求。

但是linux對筆電的電源管理不太好使的緣故。
本身是使用linux mint, 不過因為linux
mint沒有內建好的電源管理，最近因為換ssd有重灌的需求，用了幾天的發現電池使用上居然有多了50%！讓人很驚豔，寫起來日後  
要使用的話比較沒有問題。

<!-- more -->
[tlp](http://linrunner.de/en/tlp/tlp.html)的官方網站，title是linux
advanced power
managememt，安裝完後，基本上我是懶人，也都沒有做太多的設定，可以在電池上有很好的提升。

在[installation](http://linrunner.de/en/tlp/docs/tlp-linux-advanced-power-management.html#installation)裡，在ubuntu 15.10都有
內建在官方的套件庫。在以下要自己添加：

    sudo add-apt-repository ppa:linrunner/tlp
    sudo apt-get update

之後就可以安裝了：

`sudo apt-get install tlp tlp-rdw`

如果是thinkpad的話，要再多安裝：
```
 sudo apt-get install tp-smapi-dkms acpi-call-dkms 
```

安裝完之後，輸入：
`sudo tlp start` 

要查詢是否有啟動或是相關的資訊，可以用這個查詢：
```
sudo tlp stat
``` 

寫起來當筆記用，日後安裝時都要加裝tlp

### 快捷鍵設定
在Ubuntu下，台製筆電的快捷鍵好像在fn配合亮度的地方，常常會失效。目前使用幾台台廠筆電，都還要另外設定（至少asus系列的是，之前有文章說acer的也要重設）

但很簡單,用自己喜歡的編輯器（這裡用vim)：

`sudo vim /etc/default/grub`

找到這一行:

`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`
改成:
`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_osi="`


儲存離開後，下指令：
```
sudo update-grub
```


### Nvidia的顯示卡設定
```
sudo apt-get purge nvidia*
sudo apt-get purge bumblebee* primus
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-361 nvidia-prime
sudo add-apt-repository -r ppa:bumblebee/stable
```

其中nvidia-361是可以在
[官方driver](http://www.nvidia.com/Download/index.aspx?lang=en-us)
找到新版的，寫這篇文章時，是361。

### gcin設定

因為使用嘸蝦米的關係，長期以下，一直覺得在ubuntu/linux mint下，
gcin是最佳解。
[相關的安裝在這裡](http://hyperrate.com/thread.php?tid=28044)

### owncloud的設定
我有自架owncloud，應該算是個人版的dropbox
，安裝的套件在[此](https://software.opensuse.org/download/package?project=isv:ownCloud:desktop&package=owncloud-client)

