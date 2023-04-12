---
title: 在ubuntu 22.04 X64 系統 安裝32位元 軟體
slug: install-32-bit-software-in-ubuntu-2204-x64-system-zowazw
url: /post/install-32-bit-software-in-ubuntu-2204-x64-system-zowazw.html
tags: []
categories: []
lastmod: '2023-04-12 15:24:39'
toc: true
keywords: ''
description: >-
  在ubuntux系統安裝位元軟體‍今天閒著沒事想說來玩玩看在linux底下製作easyboot的隨身碟前面的步驟還挺簡單的先用gparted或者fdisk切出一個xy的空間x是你隨身碟的容量(比如說等等)y則是m(為啥是這數字？我也不知道反正是作者推薦的)​​ptnptn我是沒有切也是可以用重點是切出來的第二個partition不要去格式化然後去easyboot的網站把檔案抓下來https_wwwfosshubcomeasyboothtml我是抓這一個版本​​‍安裝ntfsg不然等一下格式化會出錯sudo
isCJKLanguage: true
---

# 在ubuntu 22.04 X64 系統 安裝32位元 軟體

‍

今天閒著沒事，想說來玩玩看在Linux底下製作easy2boot 的隨身碟

前面的步驟還挺簡單的

先用 gparted 或者 fdisk 切出一個 X-Y 的空間

X 是你隨身碟的容量(比如說32/64/128 等等)

Y 則是  600-1000M (為啥是這數字？ 我也不知道，反正是作者推薦的)

​![2023-04-12_14-58](https://raw.githubusercontent.com/changchichung/imagebed/master/202304121524805.png)​

ptn3/ptn4 我是沒有切，也是可以用，重點是切出來的第二個partition不要去格式化

---

___

然後去easy2boot 的網站把檔案抓下來

https://www.fosshub.com/Easy2Boot.html

我是抓這一個版本

​![2023-04-12_15-01](https://raw.githubusercontent.com/changchichung/imagebed/master/202304121524498.png)​

‍

安裝 ntfs-3g 不然等一下格式化會出錯

```bash
sudo apt update
sudo apt install ntfs-3g
```

抓下來並解壓縮 接著切換到剛剛解開的目錄

```bash
mkdir ~/Downloads/E2B
cd ~/Downloads/E2B && unzip ~/Downloads/Easy2Boot_v2.19_DPMS_password_is_e2b.zip  ##這邊會詢問解壓縮密碼，輸入e2b就可以了
chmod +x ~/Downloads/E2B/_ISO/docs/linux_utils/*
sudo ~/Downloads/E2B/_ISO/docs/linux_utils/fmt_ntfs.sh ##這邊會詢問你要裝在哪個分割區上，請選擇正確的分割區(就是剛剛切出來的，容量比較大的那個)
```

執行完畢之後， easy2boot 會自動把隨身碟卸載，所以要自己重插一次，或者自己 mount 回來

這時候就可以看到隨身碟裡面有剛剛複製進去的東西

然後就把ISO 複製到隨身碟底下的 _ISO 裡面的各個目錄去

接著就是我卡關的地方

本來應該要接著做 udefrag 的，但是因為這個 udefrag 還是 32位元的軟體

我的系統一開始是設定成apt 只會去抓x64軟體，所以作者建議的步驟我一直搞不定

```bash
cat ~/Downloads/E2B/_ISO/docs/linux_utils/add-32-bit-support.sh 
#!/bin/bash
# Bash Script for adding 32-bit executable support to linux 64-bit systems
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386
```

會一直出現找不到 libncurses5:i386 的錯誤

後來想到乾脆把 sources.list 清掉、回覆預設值之後再來試試看

```bash
sudo cp /etc/apt/{sources.list,sorces.list.bak} ###先備份原本的sources.list
cat <<EOF | sudo tee /etc/apt/sources.list
deb http://tw.archive.ubuntu.com/ubuntu/ jammy main universe multiverse restricted
deb http://security.ubuntu.com/ubuntu/ jammy-security main universe multiverse restricted
deb http://tw.archive.ubuntu.com/ubuntu/ jammy-updates main universe multiverse restricted
deb http://tw.archive.ubuntu.com/ubuntu/ jammy-backports main universe multiverse restricted

deb-src http://tw.archive.ubuntu.com/ubuntu/ jammy main universe multiverse restricted
deb-src http://security.ubuntu.com/ubuntu/ jammy-security main universe multiverse restricted
deb-src http://tw.archive.ubuntu.com/ubuntu/ jammy-updates main universe multiverse restricted
deb-src http://tw.archive.ubuntu.com/ubuntu/ jammy-backports main universe multiverse restricted
EOF
```

接著再跑一次 add-32-bit-support.sh，沒問題的話，應該就會安裝 libc6:i386 libncurses5:i386 libstdc++6:i386 這幾個套件了

裝完之後就依照作者的建議執行udefrag -om /dev/sxx1  這邊的sxx1 一樣是指剛剛分割出來的分割區

```bash
sudo ~/Downloads/E2B/_ISO/docs/linux_utils/udefrag -om /dev/sdd1
```

就會執行這個 udefrag 了

​![2023-04-12_15-21](https://raw.githubusercontent.com/changchichung/imagebed/master/202304121524216.png)​

‍

就放著給他跑吧！ 這時候就不得不說，還是Windows 底下方便，至少不用經過這個重整的步驟

‍
