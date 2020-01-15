---
title: "Archlinux 安裝 Part 1 - Windows & Archlinux 雙系統"
date: 2019-09-28T15:24:28+08:00
draft: false
weight: 70
keywords: ["ArchLinux", "Linux"]
description: "第二篇文章"
tags: ["ArchLinux", "Linux"]
categories: ["ArchLinux"]
author: "Huang Po-Hsun"
toc: true
series: ["linux"]
img : "images/blog/2019-09/archlinux.png"
---
2019/09/28 edited by Huang Po-Hsun

<!--![0.png](https://i.loli.net/2019/09/30/6qMYyrcQNoIm2aR.png)-->

## 前言

我分享一下如何安裝 Arch Linux 的方法，因為我之前裝 Arch Linux 時踩了很多坑，加上我是小白，所以遇到的問題很多，我會在這篇文章中教導各位如何安裝 Arch Linux 在各位的電腦，優雅地使用 Arch Linux 發行版，這篇文章是面向小白向的教學文，這篇只會提及如何安裝，至於初始化與其餘部份會另外寫篇文章。

由於我的電腦型號是**聯想 y7000p**，所以遇到的坑真的很多，我會按特殊情況講解，雖然 [Installation guide - ArchWiki](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 的安裝指南已經很詳細了，但對於剛接觸的新手根本是火星文 :( 

因為我這是專給小白寫的文章，所以寫了很多細節和注意點，如果你是高手，覺得我寫了很多廢話，那請你忍耐吧，也可以選擇不看我的文章，直接去看 ArchWiki。

## 安裝前準備

### 1. 至 [清華大學 Archlinux 軟件鏡像站](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/)下載最新版的 archlinux*.ios 鏡像

![1.png](https://i.loli.net/2019/09/29/28CjvXVRUZJzqQ3.png)

### 2. 下載 [Rufus](https://rufus.ie/) - 用來匯入 Archlinux 鏡像至 USB

![2.png](https://i.loli.net/2019/09/29/CwW9MLR5IZkSru4.png)

### 3. 準備一個至少 8G 的 USB

使用 Rufus 將 Arch 鏡像匯入在目標 USB 裝置。

> 註：在此只需修改您的目標 USB 和鏡像源即可，按下開始鍵進行匯入。

e.g.

![3.png](https://i.loli.net/2019/09/29/jR5FbkuUCPIMvXh.png)

### 4. 分割磁區

**我們假設在固態硬碟割出 40GB 空間（40GB `root`），在虛擬硬碟割出102GB 空間（2GB `swap`, 100GB `home`）**，此時別格式化割出來的磁區，讓它們保持為**未配置狀態**。

在 Windows 工作列的 `win` 圖標上點擊滑鼠右鍵選擇**磁區管理**，將自己的磁區切割，現在大部份的電腦是 110GB 左右的固態 SSD 和 1TB 左右的虛擬硬碟，當然少部份例外，我建議固態硬碟割出至少 40GB 給 `/root` 根目錄（類似 Windows的 C槽），接著可以在虛擬硬碟割出 100GB 以上給 `/home` 家目錄，也有特殊情況，像我的室友他電腦只有固態 SSD 所以只能割固態硬碟給 `/home`，我室友我幫他割了 30GB 給 `/root`，40GB 給 `/home`，2GB 給 `swap`。

> 註：我強烈不建議將 Linux 灌在 **USB** 上或是像**移動固態 SSD** 等移動裝置上，首先在啟動上很不方便，每次都要進入 BIOS 調整開機順序，又容易電腦卡頓，電腦容易發燙，加上電源的耗電速度快等因素，其實我就在我室友的移動固態 SSD 上嘗試過，甚至安裝失敗，連圖形界面都進不去。

磁區管理示意圖：

![4.png](https://i.loli.net/2019/09/29/leD4jbfzrPEIig6.jpg)

在磁區上點擊滑鼠右鍵選擇**壓縮磁碟**，應你的需求壓縮適當大小的磁區。

### 5. USB 使用 UEFI 方式開機

**進入 BIOS，調整 Boot 開機順序，改為你的 USB 優先開機，主機與 USB 開機模式為 UEFI**

每台電腦進入 BIOS 的方法都不同，像我的電腦是聯想 y7000p，所以我在開機時一直按住 `Fn + F2` ，就能進入 BIOS 調整開機順序，因為幫室友們裝了 Arch，所以 ThinkPad 是 `F1`，華碩是按 `Esc` 或 `F2`，小米是 `F2`，我記得小米筆電進入 BIOS 還需要輸入 Password，有點麻煩就是了。當然還是勸大家上網搜一下自己電腦型號的進入 BIOS 快捷建。不好意思，我因為 linux 無法掛載我的移動 SSD，具體是因為我之前想將 arch 灌到 SSD 上，後來覺得使用上麻煩所以放棄了， 所以我先將該磁區刪除再新增，

成功進入 USB 後選擇第一個選項 `Boot Arch Linux (X86_64)` 按 `Enter` 進入，當你看到 `root #` 就代表你已經進入 USB 裡的 Arch 安裝鏡像。

> 註：切記要是 **UEFI 開機**，因為我們最後要在 BIOS 安裝引導程序，來引導我們開機選擇進入的 OS。如果沒有使用 UEFI 開機的話會裝不上引導程序 `grub`。

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

> 註：在這裡請務必確認你的 `/boot` 是掛載在 `EFI Syetem` 分區上，不然後面需要進入 `Chroot` 執行 `pacman -S linux` 重裝系統。

```zsh
ping baidu.com # 確認此時你是連上網的
pacstrap /mnt base linux linux-firmware # 安裝基本系統
```

如果你是第二次裝 ArchLinux 的小夥伴，你執行以上指令可能會遇到下載完後安裝時，顯示 `/boot/vmlinuz-linux` 已存在，所以無法安裝，這時執行以下指令。

```zsh
rm /boot/vmlinuz-linux # 刪除該檔案
pacstrap /mnt base linux linux-firmware # 安裝基本系統
```

> 註：pacstrap 的安裝指令我在 2020/01/04 更新，因為幫同學重灌 ArchLinux 時發現指令變了，目前最新本的 ArchLinux 發行版指令是 `pacstrap /mnt base linux linux-firmware`。

## 配置系統

### 1. Fastab

執行以下指令生成 `fstab` 文件（用 `-U` 或 `-L` 選項設置 UUID 或卷標）。

```zsh
genfstab -U /mnt >> /mnt/etc/fstab # 生成 fstab 文件
```

**我個人強烈建議生成完 `fstab` 後使用 Vim 查看內容，尤其是重裝 Arch 和重新分配磁區的人一定要查看這份文件。**

> 註：我之前因為將 `/root` 掛載在虛擬硬碟，所以在 Linux 環境下電腦特別卡，在重灌一次後因為沒有重新砍掉 `fstab` 文件生成一次，所以上一次安裝時的設定即使執行生成 `fstab` 文件指令，還是會保留設定無法去除，只是添加了設定，無法覆蓋源設定，造成第二次還是那麼卡。

```zsh
vim /mnt/etc/fstab # 查看 fstab 文件
```

請確認這份文件裡只有四份資料（如果有多分割幾個磁區，就可能不是只有四份，你掛載幾個目錄就有幾份資料，不能多不能少）。

![table.png](https://i.loli.net/2019/10/01/S9YdZyhaDmctb1R.png)

![15.png](https://i.loli.net/2019/09/30/cnKvMBOuLbl7x9Q.png)

如果你的文件有問題且資料混亂執行以下指令。

```zsh
rm /mnt/etc/fstab # 移除原文件
genfstab -U /mnt >> /mnt/etc/fstab # 生成 fstab 文件
```

再次用 Vim 確認無誤之後，就可進入下一步。

### 2. 進入 Chroot

`Change root` 到新安裝的系統，顧名思義就是進入電腦以安裝好的系統，目前我們所在的地方是 USB，如果沒進入 `Chroot` 就進行安裝和設定就等於安裝和設定在你的 USB 上。

> 註：如果重裝系統或是需要重新插上 USB 做些設定，像我通常就發生在磁區分割錯誤或目錄掛載錯誤或 Windows 系統更新後引導程序設定檔被砍，需要先將所有的分區先掛載，再進入 `Chroot` 才會顯示你電腦裡的磁區，**再次強調，先掛載 `/mnt`**。

```zsh
arch-chroot /mnt # 進入 Chroot
```

### 3. 時區

設置時區，`Region` 為你所在的洲，`City` 為你所在城市。

```zsh
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime # 設置時區
```

我設定的是台北時間。

```zsh
ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime
```

執行 ` hwclock` 生成 `/etc/adjtime`。

```zsh
hwclock --systohc # 生成 /etc/adjtime
```

這個指令假定硬件時間已經設置為 `UTF` 時間。

### 4. 添加本地語系

編輯 `locale.gen` 把自己要用的語言註解去掉，也就是去掉開頭的 `#`，一定要取消註解 `en_US.UTF-8 UTF-8`，建議取消註解**帶 `UTF-8` 的語言**，**把 GBK 和 BIG5 註解去掉可以支援更多字**。

```zsh
nano /etc/locale.gen # 編輯 locale.gen
```

以下是我要取消註解的語言，因為我繁體字和簡體字都很常用。

```vim
en_US.UTF-8 UTF-8
zh_CN.GB18030 GB18030
zh_CN.GBK GBK
zh_TW.EUC-TW EUC-TW
zh_TW.UTF-8 UTF-8
```

執行 `locale-gen` 生成 `locale` 訊息。

```zsh
 locale-gen # 生成 locale 訊息
```

接著創建 `locale.conf` 檔案並編輯 `LANG` 這一個**語言環境**環境變量為英語 `en_US.UTF-8`，如果你設為中文的話會造成 tty 出現亂碼。

```zsh
vim /etc/locale.conf # 創建並編輯 locale.conf
```

修改語言環境。

```vim
LANG=en_US.UTF-8 # 變量設為英語
```

### 5. 主機名稱與網路

創建 `hostname` 文件設置你的主機名稱。

```zsh
vim /etc/hostname # 創建 hostname 文件
```

直接在 `hostname` 填入你主機名稱，`myhostname`是你的主機名。

```vim
myhostname # 你的主機名稱
```

添加對應訊息到 `hosts(5)`。

```zsh
vim /etc/hosts # 編輯 hosts
```

寫入以下內容，將 `myhostname` 修改成你前面設定的主機名稱。

> 註：如果系統有一個永久的 IP 地址，請使用這個永久的 IP 地址，而不是 `127.0.1.1`。
> 
> 像我就沒有設永久 IP，所以直接編輯成以下內容。

```vim
127.0.0.1	localhost
::1		localhost
127.0.1.1	myhostname.localdomain	myhostname
```

### 6. Initramfs

創建 `Initramfs`，這是用來將內存盤初始化的腳本，例如開關機時掛載與卸載磁區，現在 `Initramfs`已取代了舊版的 `initrd`。

```zsh
mkinitcpio -p linux # 創建 Initramfs
```

如果你想深入了解 `Initramfs`，可以參考 [mkinitcpio (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Mkinitcpio_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 和 [Initramfs/指南 - gentoo linux](https://wiki.gentoo.org/wiki/Initramfs/Guide/zh-cn)。

### 7. 設定 Root (超級使用者) 密碼

設定 `Root` 超級使用者的密碼，**超級使用者**的意思就是這個用戶相當於**系統管理員**，擁有修改系統設定的所有權力。

輸入以下指令後會要求你輸兩次密碼，第二次是確認你的密碼正確，**輸入密碼時文字是隱藏的**，所以不用慌張以為沒有輸入進去。

```zsh
passwd # 設定密碼
```

### 8. 安裝引導程序

因為我之前裝的引導程序是 `rEFInd`，但 `rEFInd` 實在太不穩了，好幾次隨著 Windows 更新而設定檔被砍進不了  Linux，需要重裝，然後我朋友推薦給我使用**直接裝在 BIOS 的 `grub`**，穩定不容易被砍，雖然界面比 `rEFInd` 丑了很多，但至少穩定。

`grub` 是引導程序，`os-prober`是可以偵測其他操作系統的軟體包，`efibootmgr`可以操控 UEFI 固件啟動管理器設置的工具。

> 註：請確定你是**以 UEFI 模式開機**，不然會一直報錯 `efi variables are not supported on this system`。這時你就必須重新開機進入 BIOS 調整開機模式再進到 Arch Linux 安裝碟安裝 grub。也請確定你的 **Boot 是掛載在 EFI System 磁區**，不然你無法產生 grub 設定檔，原因在於我們是要掛載在 Boot，但因為 Boot 沒掛載到 EFI 磁區，所以在 EFI 磁區找不到 Boot，產生路徑錯誤。

```zsh
sudo pacman -Syu # 現在更新一下系統
sudo pacman -Syy # 同步數據庫
sudo pacman -S grub os-prober efibootmgr # 下載軟體包
```
接著把 `grub` 安裝到 **EFI System 磁區**，將以下指令`--efi-directory` 的 `esp` 改為你的 `grub` 掛載點，我們要將 `grub`  掛載到 `/boot` 上，所以 **`esp` 改為 `/boot`**。

* 指令格式

```zsh
 grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=grub
```

* 我的掛載方式

```zsh
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
```

然後**掛載**你的 Windows 磁區，你的固態硬碟 C 槽磁區，沒掛載的話到時候產生設定檔會找不到你的 Windows。

```zsh
mkdir /mnt/windows # 創建 C 槽目錄
mount /dev/sdaX1 /mnt/windows # 掛載到目標磁區
```

當然如果你還想讀取其它 Windows 的磁區，你可以現在順便掛載好，當然後面再掛載也行。

現在我想將我的虛擬硬碟 Wiindows 的 D 槽掛載。

```zsh
mkdir /mnt/Data # 創建 D 槽目錄
mount /dev/sdaX2 /mnt/Data # 掛載到目標磁區
```

然後產生 `grub` 設定檔。

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

確認設定檔產生無報錯後接著退出 `Chroot`。

```zsh
exit # 退出 Chroot
```

## 重要補充

### 連網

因為拔掉 USB 重啟電腦後，會發現關於**連網功能不好處理**，使用 `ip link` 後 `dhcpcd` 過於繁瑣，建議就在這裡就安裝連網的管理器。

> 註：注意使用 `systemctl` 啟用 **networkmanager** 服務時要注意大小寫，開頭要大寫。

```zsh
arch-chroot /mnt # 進入 Chroot
pacman -S networkmanager # 安裝 networkmanager
systemctl enable NetworkManager # 設定開機自啟
systemctl strat NetworkManager # 啟用 Netmanager
exit # 退出 Chroot
```

### 不裝雙系統，只有 Linux

如果你是將所有電腦裡的系統都砍了只裝 Archlinux，你**一樣要裝引導**，像之前我幫一個朋友裝 ArchLinux 時他將自己電腦上的 **Windows 全砍了**，剩下所有空間都給 Linux。

## 重啟電腦

因為我手速不夠快，如果不小心在還沒完全關機或已經開機狀態下拔下 USB，會造成嚴重錯誤，所以我選擇直接關機再手動開機。

```zsh
reboot # 重開機
poweroff # 直接關機
```

關機後立刻將 USB 拔出電腦，然後開機，如果開機沒有進入 grub，重開機一次進入 BIOS 會看到 有 `grub` 的開機選項，將這個選項調到最優先開機就行了。

> 登入時輸入 `user` 為 `root`，`password` 為你之前設的 Root 密碼。

## 安裝後工作

我會之後會寫 **Arch Linux 安裝後**文章，並放入 Blog。

## Reference

* [Installation guide (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Installation_guide_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [超簡明 Vim 操作介紹 - 為你自己學 Git](https://gitbook.tw/chapters/command-line/vim-introduction.html)
* [请问vim如何移动当前行向上或向下？不用选中 - V2EX](https://www.v2ex.com/t/49043)
* [EFI system partition (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/EFI_system_partition_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [Network configuration (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Network_configuration_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [mkinitcpio (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Mkinitcpio_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [Initramfs/指南 - gentoo linux](https://wiki.gentoo.org/wiki/Initramfs/Guide/zh-cn)
* [Localization/Simplified Chinese (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Localization/Simplified_Chinese_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [Unified Extensible Firmware Interface (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
* [GRUB (简体中文) - ArchWiki](https://wiki.archlinux.org/index.php/GRUB_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E7%94%9F%E6%88%90_grub.cfg)

##### &copy;copyright by Huang Po-Hsun