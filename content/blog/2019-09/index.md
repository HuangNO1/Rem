---
title: "Archlinux 安裝 - Windows & Archlinux 雙系統"
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

![0.png](https://i.loli.net/2019/09/30/6qMYyrcQNoIm2aR.png)

## 前言

我分享一下如何安裝 Arch Linux 的方法，因為我之前裝 Arch Linux 時踩了很多坑，加上我是小白，所以遇到的問題很多，我會在這篇文章中教導各位如何安裝 Arch Linux 在各位的電腦，優雅地使用 Arch Linux 發行版，這篇文章是面向小白向的教學文，這篇只會提及如何安裝，至於初始化與其餘部份會另外寫篇文章。

由於我的電腦型號是**聯想 y7000p**，所以遇到的坑真的很多，我會按特殊情況講解，雖然 [Installation guide - ArchWiki](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 的安裝指南已經很詳細了，但對於剛接觸的我根本是火星文 :( 

## 安裝前準備

### 1. 至 [清華大學 Archlinux 軟件鏡像站](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/)下載最新版的 archlinux*.ios 鏡像

![1.png](https://i.loli.net/2019/09/29/28CjvXVRUZJzqQ3.png)

### 2. 下載 [Rufus](https://rufus.ie/) - 用來匯入 Archlinux 鏡像至 USB

![2.png](https://i.loli.net/2019/09/29/CwW9MLR5IZkSru4.，png)

### 3. 準備一個至少 8G 的 USB

使用 Rufus 將 Arch 鏡像匯入在目標 USB 裝置。

> 註：在此只需修改您的目標 USB 和鏡像源即可，按下開始鍵進行匯入。

e.g.

![3.png](https://i.loli.net/2019/09/29/jR5FbkuUCPIMvXh.png)

### 4. 分割磁區

**在固態硬碟割出 40GB 空間（40GB `root`），在虛擬硬碟割出102GB 空間（2GB `swap`, 100GB `home`）**，此時別格式化割出來的磁區，讓它們保持為**未配置狀態**。

在 Windows 工作列的 `win` 圖標上點擊滑鼠右鍵選擇**磁區管理**，將自己的磁區切割，現在大部份的電腦是 110GB 左右的固態 SSD 和 1TB 左右的虛擬硬碟，當然少部份例外，我建議固態硬碟割出至少 40GB 給 `/root` 根目錄（類似 Windows的 C槽），接著可以在虛擬硬碟割出 100GB 以上給 `/home` 家目錄，也有特殊情況，像我的室友他電腦只有固態 SSD 所以只能割固態硬碟給 `/home`，我室友我幫他割了 30GB 給 `/root`，40GB 給 `/home`，2GB 給 `swap`。

> 註：我強烈不建議將 Linux 灌在 **USB** 上或是像**移動固態 SSD** 等移動裝置上，首先在啟動上很不方便，每次都要進入 Bios 調整開機順序，又容易電腦卡頓，電腦容易發燙，加上電源的耗電速度快等因素，其實我就在我室友的移動固態 SSD 上嘗試過，甚至安裝失敗，連圖形界面都進不去。

磁區管理示意圖：

![4.png](https://i.loli.net/2019/09/29/leD4jbfzrPEIig6.jpg)

在磁區上點擊滑鼠右鍵選擇**壓縮磁碟**，應你的需求壓縮適當大小的磁區。

### 5. USB 使用 UEFI 方式開機

**進入 Bios，調整 Boot 開機順序，改為你的 USB 優先開機，開機模式為 UEFI**

每台電腦進入 Bios 的方法都不同，像我的電腦是聯想 y7000p，所以我在開機時一直按住 `Fn + F2` ，就能進入 Bios 調整開機順序，因為幫室友們裝了 Arch，所以 ThinkPad 是 `F1`，華碩是按 `Esc` 或 `F2`，小米是 `F2`，我記得小米筆電進入 Bios 還需要輸入 Password，有點麻煩就是了。當然還是勸大家上網搜一下自己電腦型號的進入 Bios 快捷建。

成功進入 USB 後選擇第一個選項 `Boot Arch Linux (X86_64)` 按 `Enter我們的` 進入，當你看到 `root #` 就代表你已經進入 USB 裡的 Arch 安裝鏡像。

> 註：切記要是 UEFI 開機，因為我們最後要在 Bios 安裝引導程序，來引導我們開機選擇進入的 OS。如果沒有使用 UEFi 開機的話會裝不上引導程序 `grub`。

![5.png](https://i.loli.net/2019/09/30/q4zMECx7tpkNLK1.png)

### 6. 確認連到網路

在安裝過程我們需要用到網路，我們需要確認我們連的到網路，輸入以下指令

```zsh
ping baidu.com # 確認是否可連上百度
```

如果你的結果與下圖相同則代表你已連上了網路，按下 `Ctrl + C` 中止命令，

![6.png](https://i.loli.net/2019/09/29/2XAfjoPpSaJDu4d.png)

如果無法從 `baidu.com` 接收 `packets`，**掉包率（packets loss）100%**，代表你沒有連上網路，這時我建議插網線連有線網路，不建議連無線網路，因為在無線網路使用上會出些問題，不建議新手使用，我最常用的方法是將使用**數據線**將電腦與手機連結，然後手機開 **USB 共用網路**，手機可以連行動數據或 WiFi 給電腦網路。

1.插上網線

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

2.用數據線將手機與電腦連結，並開啟 USB 共用網路

輸入 `ip link` 後出現了新的有線接口 `enp0s20f0u1`，此為你的手機網路接口。

> 註：通常自己電腦的有線網路接口名稱比手機的網路接口名稱短。

![8.png](https://i.loli.net/2019/09/29/RmvsnrfoKt5caUH.png)

```zsh
ip link set enp7s0 down # 關閉自己電腦的有線接口
ip link set enp0s20f0u1 up # 開啟手機的有線接口
dhcpcd enp0s20f0u1 # 連接手機的有線接口
ping baidu.com # 測試是否連上網路
```

通常到這裡網路是連上的，如果還連不上那可能就要連無線 WiFi 了，我就不多說明了，可以參考 [Wireless network configuration (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Wireless_network_configuration_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 和 [某網友的 Blog](http://irislr.me/2017/01/07/linux%E7%B3%BB%E7%BB%9F%E7%9A%84%E6%9C%89%E7%BA%BF%EF%BC%86%E6%97%A0%E7%BA%BF%E8%BF%9E%E6%8E%A5/) 和 [某教程網](https://www.itread01.com/p/149560.html) 有提及，因為之前我幫我朋友安裝 Archlinux 時要連無線 WiFi 時搞了很久，還出了錯誤，最後直接使用手機的共用網路才解決。

### 7. 更新系統時間

使用 `timedatectl` 確保系統時間是準確的，後面我們在**同步數據庫**時需要系統時間與網路時間的同步。

> 註：這一步真的很重要，我之前因為忘了這一步驟導致數據庫無法同步，開啟時間同步後還要進入 `Chroot` 重裝一次系統 `pacman -S linux`

```zsh
timedatectl set-ntp true # 開啟時間同步
timedatectl status # 檢查服務狀態
```

![9.png](https://i.loli.net/2019/09/30/C5vDyeu6cOnIgs4.png)

請確保 `NTP service` 狀態為 `active`，且確定自己的系統時間是否正確。

### 8. 分割磁區

**我們使用 `cfdisk` 來分割磁區**

ArchWiki 建議的分區參考

![10.png](https://i.loli.net/2019/09/30/dQUhfT6SNIAPq4D.png)

我們已經在 Windows 分割好了我們的磁區。

```zsh
lsblk # 檢查磁區狀態
```

![11.png](https://i.loli.net/2019/09/30/NMrOdjfom7FBA54.png)

以上是我的磁區狀態，因為我是已經裝好了系統，所以會顯示這樣的狀態，但你現在的狀態是不會顯示出你切割出來的磁區，這裡**記得你的磁區大小**以便在這裡辨認出你的磁區哪些是固態硬碟和虛擬硬碟。

> 註：有些指南標示 `/dev/sdX` 的 `sdX` 意思是你的磁區代號，有些電腦的固態硬碟代號是 `sdb` 或是 `nvme0n1`，虛擬硬碟通常代號是 `sda`，你可以將 `sdX` 當成一個代稱，因為沒人保證你電腦裡的硬碟的代號為何，像我有個室友他電腦的固態硬碟是 `sdb`。
>
> 在輸入路徑時可以搭配 `Tab` 鍵快速輸入，輸入開頭先按一次 `Tab`，如果指令行下出現許多路徑可按兩次 `Tab` 選取。

**在這裡我們假設你的固態硬碟是 `nvme0n1` - 割出 40GB 給 `root`，虛擬硬碟是 `sda` - 割出 102GB**。輸入以下指令進入管理磁區界面。

```zsh
cfdisk /dev/nvme0n1 # 管理固態硬碟磁區
```

![12.png](https://i.loli.net/2019/09/30/Mm78ohqWiusweDQ.png)

這裡是管理你的固態硬碟界面。先使用上下鍵調到你割出來的 `Free Space` 40GB，左右鍵調到 `New` 按下 `Enter`，輸好要分配出的大小再按 `Enter` 將空間分配出來，接著左右鍵調到 `Type`，**選取 `Linux root (ARM-64)`**，這是 64 bit 的系統，如果你的電腦是 32 bit 請選擇 `Linux root (X86)`，接著**確認你的磁區上面有個磁區類型是 `EFI System` ，並記住磁區代號或大小**，這裡是要掛載 `boot` 的磁區，如果沒有的話請找到可以自己在割出大概從某個磁區割出 250MB 調整類型為 `EFI System` 分區。接著左右鍵調到 `Write` 寫入設定，輸入 `yes` 按下 `Enter`，確認無誤後可以按下 `Quit` 離開此界面，再次 輸入 `lsblk` 就可以在 `/dev/nvme0n1` 看到你分割出的空間了。

> 註：記得一定要寫入，我之前很多次都忘了寫入，重搞了幾次。

```zsh
cfdisk /dev/sda # 管理虛擬硬碟磁區
```

示意圖我就不放了，方式都跟上個步驟大同小異，分別割出 2GB `Type` 為 `Linux Swap` 和 100GB `Type` 為 `Linux home` 的分區。

> 註：有些人的電腦可能沒有我上述講的類型，如果找不到 `Linux root (ARM-64)`、`Linux root (X86)`、`Linux home`，可以將 `Type` 改成 `Linux filesystem`，只是 `EFI System` 與 `Linux swap` 不能這樣搞，而且這兩類型是找得到的。

### 9. 格式化磁區

當分區建好後，我們需要對此格式化。每種類型的磁區格式化方式都不相同。

> 註：我們假設你分割出的 40GB 固態硬碟空間磁區代號為 `nvme0n1p5`，`boot` 為 `nvme0n1p1`，2GB `Swap` 為 `sda3` ，100GB `home` 為 `sda4`。
>
> 註：`mkfs.ext4` 的創建方式與 `mkfs.fat` 的差別在於前者有創建 `journal` 日誌，後者沒有。注意：文件系統一定要創建日誌，不然個人的資料可能會造成損失等後果。

* 文件系統 `root` 與 `home`

```zsh
mkfs.ext4 /dev/nvme0n1p5 #格式化 root
mkfs.ext4 /dev/sda4 #格式化 home
```

* EFI 系統分區 `boot`

```zsh
mkfs.fat -F32 /dev/nvme0n1p1 # 格式化 boot
```

* 置換空間 `swap`

```zsh
mkswap /dev/sda3 # 格式化 swap
swapon /dev/sda3 # 掛載置換空間
```

### 10. 掛載分區

因為根目錄 `/mnt` 已在你的 USB 存在，所以不需要創建。**注意：必須優先掛載 `/mnt` 再掛載其它目錄**。

```zsh
mount /dev/nvme0n1p5 /mnt # 掛載根目錄 root
mkdir /mnt/boot # 創建 /boot 目錄
mount /dev/nvme0np1 /mnt/boot # 掛載 /boot
mkdir /mnt/home # 創建 /home 目錄
mount /dev/sda4 /mnt/home # 掛載 /home
lsblk # 查看分區無誤
```

掛載好後，接下來 `genfstab` 將會自動檢測掛載的文件系統和置換空間。

## 安裝

### 1. 選擇鏡像

編輯 `/etc/pacman.d/mirrorlist` 文件，將你的所在的鏡像地址優先排在前面，在列表中越前面的地址修先級越高。

```zsh
vim /etc/pacman.d/mirrorlist # 編輯文件
```

關於 Vim 的編輯指令可參考 [超簡明 Vim 操作介紹](https://gitbook.tw/chapters/command-line/vim-introduction.html)，用起來其實不難。

![13.png](https://i.loli.net/2019/09/30/tj7PcF9vMVKZ3Bh.png)

關於 Vim 移動整行的指令

```vim
:12, 13 move 6 # 將第 12, 13 行剪貼至第 6 行
```

像我目前所在的地區為 China，所以我將 China 的鏡像源都移至最開頭。

![14.png](https://i.loli.net/2019/09/30/8jfCaXqbSOEWT5M.png)

編輯好並確認無誤後退出 Vim 編輯器。

### 2. 安裝基本系統

使用 `pacstrap` 腳本安裝 `base` 組：

> 註：在這裡請勿必確認你的 `/boot` 是掛載在 `EFI Syetem` 分區上，不然後面需要進入 `Chroot` 執行 `pacman -S linux` 重裝系統。

```zsh
ping baidu.com # 確認此時你是連上網的
pacstrap /mnt base # 安裝基本系統
```

如果你是第二次裝 ArchLinux 的小夥伴，你執行以上指令可能會遇到下載完後安裝時，顯示 `/boot/vmlinuz-linux` 已存在，所以無法安裝，這時執行以下指令。

```zsh
rm /boot/vmlinuz-linux # 刪除該檔案
pacstrap /mnt base # 安裝基本系統
```

## 配置系統

### 1. Fastab

執行以下指令生成 `fstab` 文件（用 `-U` 或 `-L` 選項設置 UUID 或卷標）

```zsh
genfstab -U /mnt >> /mnt/etc/fstab # 生成 fstab 文件
```

**我個人強烈建議生成完 `fstab` 後使用 `Vim` 查看內容，尤其是重裝 Arch 和重新分配磁區的人一定要查看這份文件。**

> 註：我之前因為將 `/root` 掛載在虛擬硬碟，所以在 Linux 環境下電腦特別卡，在重灌一次後因為沒有重新砍掉 `fstab` 文件生成一次，所以上一次安裝時的設定即使執行生成 `fstab` 文件指令，還是會保留設定無法去除，只是添加了設定，無法覆蓋源設定，造成第二次還是那麼卡。

```zsh
vim /mnt/etc/fstab # 查看 fstab 文件
```

請確認這份文件裡只有四份資料（如果有多分割幾個磁區，就可能不是只有四份，你掛載幾個目錄就有幾份資料，不能多不能少）

|           file system           |                    dir                    | type  | options | dump                                                         | pass |
| :-----------------------------: | :---------------------------------------: | :---: | :-----: | :----------------------------------------------------------- | :--: |
|         /dev/nvme0n1p5          | UUID=a48c597a-d1f2-4f4a-82f8-ba96114912f1 |   /   |  ext4   | rw,relatime                                                  | 0 1  |
|            /dev/sda3            | UUID=bcdb5959-181b-4a68-94e3-f5b79c0a14a8 | /home |  ext4   | rw,relatime                                                  | 0 2  |
| /dev/nvme0n1p1 LABEL=SYSTEM_DRV |              UUID=1258-CD76               | /boot |  vfat   | rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,utf8,errors=remount-ro | 0 2  |
|            /dev/sda4            | UUID=867cc6ea-05af-4676-b36f-875dd7570a38 | none  |  swap   | defaults                                                     | 0 0  |

![15.png](https://i.loli.net/2019/09/30/cnKvMBOuLbl7x9Q.png)

如果你的文件有問題且資料混亂執行以下指令。

```zsh
rm /mnt/etc/fstab # 移除原文件
genfstab -U /mnt >> /mnt/etc/fstab # 生成 fstab 文件
```

再次用 `Vim` 確認無誤之後，就可進入下一步

### 2. 進入 Chroot

`Change root` 到新安裝的系統，顧名思義就是進入電腦以安裝好的地址修先級越高。

```zsh
vim /etc/pacman.d/mirrorlist # 編輯文件
```

關於 Vim 的編輯指令可參考 [超簡明 Vim 操作介紹](https://gitbook.tw/chapters/command-line/vim-introduction.html)，用起來其實不難。

![13.png](https://i.loli.net/2019/09/30/tj7PcF9vMVKZ3Bh.png)

關於 Vim 移動整行的指令

```vim
:12, 13 move 6 # 將第 12, 13 行剪貼至第 6 行
```

像我目前所在的地區為 China，所以我將 China 的鏡像源都移至最開頭。

![14.png](https://i.loli.net/2019/09/30/8jfCaXqbSOEWT5M.png)

編輯好並確認無誤後退出 Vim 編輯器。系統，目前我們所在的地方是 USB，如果沒進入 `Chroot` 就進行安裝和設定就等於裝在你的 USB 上。

> 註：如果重裝系統或是需要重新插上 USB 做些設定設定，像我通常就發生在磁區分割錯誤或目錄掛載錯誤或 Windows 系統更新後引導程序設定檔被砍，需要先將所有的分區先掛載，再進入 `Chroot` 才會顯示你電腦裡的磁區，**再次強調，先掛載 `/mnt`**。

```zsh
arch-chroot /mnt # 進入 Chroot
```

### 3. 時區

設置時區，

```zsh
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime # 設置時區
```

### 4. 添加本地語系

### 5. 主機名稱與網路

### 6. Initramfs

### 7. 設定 Root (超級使用者) 密碼

## 重啟電腦


## Reference

* [Installation guide (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [超簡明 Vim 操作介紹 - 為你自己學 Git](https://gitbook.tw/chapters/command-line/vim-introduction.html)
* [请问vim如何移动当前行向上或向下？不用选中 - V2EX](https://www.v2ex.com/t/49043)
* [EFI system partition (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/EFI_system_partition_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [Network configuration (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Network_configuration_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [Initramfs/指南 - gentoo linux](https://wiki.gentoo.org/wiki/Initramfs/Guide/zh-cn)



##### &copy;copyright by Huang Po-Hsun

