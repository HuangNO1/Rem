---
title: "Archlinux 安裝"
date: 2019-09-28T15:24:28+08:00
draft: false
weight: 70
keywords: ["ArchLinux", "Linux"]
description: "第二篇文章"
tags: ["ArchLinux", "Linux"]
categories: ["ArchLinux"]
author: "Huang Po-Hsun"
toc: true
series: ["Linux"]
---
2019/09/28 edited by Huang Po-Hsun

![0.png](https://i.loli.net/2019/09/29/4GuroyLhaBXediw.png)

## 前言

我分享一下如何安裝 Arch Linux 的方法，因為我之前裝 Arch Linux 時踩了很多坑，加上我是小白，所以遇到的問題很多，我會在這篇文章中教導各位如何安裝 Arch Linux 在各位的電腦，優雅地使用 Arch Linux 發行版，這篇文章是面向小白向的教學文，這篇只會提及如何安裝，至於初始化與其餘部份會另外寫篇文章。

由於我的電腦型號是**聯想 y7000p**，所以遇到的坑真的很多，我會按特殊情況講解，雖然 [Arch WIKI Installation guide](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 的安裝指南已經很詳細了，但對於剛接觸的我根本是火星文 :( 

## 安裝前準備

### 1. 至 [清華大學 Archlinux 軟件鏡像站](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/)下載最新版的 `archlinux*.ios` 鏡像

![1.png](https://i.loli.net/2019/09/29/28CjvXVRUZJzqQ3.png)

### 2. 下載 [Rufus](https://rufus.ie/) - 用來匯入 Archlinux 鏡像至 USB

![2.png](https://i.loli.net/2019/09/29/CwW9MLR5IZkSru4.png)

### 3. 準備一個至少 8G 的 USB

使用 Rufus 將 Arch 鏡像匯入在目標 USB 裝置。

> 註：在此只需修改您的目標 USB 和鏡像源即可，按下開始鍵進行匯入。

e.g.

![3.png](https://i.loli.net/2019/09/29/jR5FbkuUCPIMvXh.png)

### 4. 分割磁區

**在固態硬碟割出 40GB 空間（40GB `root`），在虛擬硬碟割出102GB 空間（2GB `swap`, 100GB `home`）**，此時別格式化割出來的磁區，讓它們保持為**未配置狀態**。

在 Windows 工作列的 `win` 圖標上點擊滑鼠右鍵選擇**磁區管理**，將自己的磁區切割，現在大部份的電腦是 110GB 左右的固態 SSD 和 1TB 左右的虛擬硬碟，當然少部份例外，我建議固態硬碟割出至少 40GB 給 `/root` 根目錄（類似 Windows的 C槽），接著可以在虛擬硬碟割出 100GB 以上給 `/home` 家目錄，當然也有特殊情況，像我的室友他電腦只有固態 SSD 所以只能割固態硬碟給 `/home`，我室友我幫他割了 30GB 給 `/root`，40GB 給 `/home`，2GB 給 `swap`。

> 註：我強烈不建議將 Linux 灌在 **USB** 上或是像**移動固態 SSD** 等移動裝置上，首先在啟動上很不方便，每次都要進入 Bios 調整開機順序，又容易電腦卡頓，電腦容易發燙，加上電源的耗電速度快等因素，其實我就在我室友的移動固態 SSD 上嘗試過，甚至安裝失敗，連圖形界面都進不去。

磁區管理示意圖：

![4.png](https://i.loli.net/2019/09/29/leD4jbfzrPEIig6.jpg)

在磁區上點擊滑鼠右鍵選擇**壓縮磁碟**，應你的需求壓縮適當大小的磁區。

### 5. USB 使用 UEFI 方式開機

**進入 Bios，調整 Boot 開機順序，改為你的 USB 優先開機，開機模式為 UEFI**

每台電腦進入 Bios 的方法都不同，像我的電腦是聯想 y7000p，所以我在開機時一直按住 `Fn + F2` ，就能進入 Bios 調整開機順序，因為幫室友們裝了 Arch，所以 ThinkPad 是 `F1`，華碩是按 `Esc` 或 `F2`，小米是 `F2`，我記得小米筆電進入 Bios 還需要輸入 Password，有點麻煩就是了。當然還是勸大家上網搜一下自己電腦型號的進入 Bios 快捷建。

成功進入 USB 後選擇第一個選項 `Boot Arch Linux (X86_64)` 按 `Enter` 進入，當你看到 `root #` 就代表你已經進入 USB 裡的 Arch 安裝鏡像。

> 註：切記要是 UEFI 開機，因為我們最後要在 Bios 安裝引導程序，來引導我們開機選擇進入的 OS。如果沒有使用 UEFi 開機的話會裝不上引導程序 `grub`。

![5.png](https://imgpoi.com/i/71GFNM.png)

### 確認連到網路

在安裝過程我們需要用到網路，我們需要確認我們連的到網路，輸入以下指令

```zsh
ping baidu.com # 確認是否可連上百度
```

如果你的結果與下圖相同則代表你已連上了網路，按下 `Ctrl + C` 中止命令，

![6.png](https://i.loli.net/2019/09/29/2XAfjoPpSaJDu4d.png)

如果無法從 `baidu.com` 接收 `packets`，**掉包率（packets loss）100%**，代表你沒有連上網路，這時我建議插網線連有線網路，不建議連無線網路，因為在無線網路使用上會出些問題，不建議新手使用，我最常用的方法是將使用**數據線**將電腦與手機連結，然後手機開 **USB 共用網路**，手機可以連行動數據或 WiFi 給電腦網路。

1. 插上網線

```zsh
ip link #顯示自己的網路接口
```

![7.png](https://i.loli.net/2019/09/29/aILSzq6P5vWg8uG.png)

`enp7s0` 是我的有線接口，`wlp0s20f3` 是我的無線接口，當然這些接口名稱會隨著電腦不同而不同但大多相似。

```zsh
ip link set enp7s0 up # 將有線接口打開
dhcpcd enp7s0 # 連接接口
ping baidu.com # 再測試一次是否連網
```

以上如果沒問題就可跳過用手機共用網路步驟

2. 用數據線將手機與電腦連結，並開啟 USB 共用網路
![8.png](https://i.loli.net/2019/09/29/RmvsnrfoKt5caUH.png)






























































##### &copy;copyright by Huang Po-Hsun

