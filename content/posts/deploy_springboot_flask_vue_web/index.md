---
title: "部署 SpringBoot、Flask 與 Vue 前端與雙後端完全分離 Web 項目"
date: 2020-12-21T16:49:28+08:00
# 副標題
subtitle: ""
# 上次修改的時間
lastmod: 2020-12-21T16:49:26+08:00
draft: false
description: ""
license: ""
tags: ["Vue", "Vue-cli", "Web", "npm", "SpringBoot", "Linux", "Java", "JDK", "Maven", "IntelliJ", "IDEA", "Jar", "Nginx", "Python", "Flask"]
categories: ["Linux"]
featuredImage: "/featuredImage/compressed/deploy_springboot_flask_vue_web.png"
---

## 前言

由於這學期我選修了軟件測試的課程，我們以十人小組為單位，對我在暑假實訓所參與的 **MD-Blog 個人空間系統**項目進行 Web 測試，過程中我將那個項目部署到了**阿里雲的雲服務器**上。這篇文章我紀錄一下我部署 Web 的過程。

## 購買雲服務器

因為我是用**學生優惠**等方式白嫖到了半年的阿里雲雲服務器，所以這方面其實沒怎麼出錢，幸好在軟件測試項目結題才到期，不過用學生優惠購買最低配的雲服務器，三個月也只需要 28 元人民幣，騰訊雲我因為過不了實名認證就算了（...這裡我要吐嘈一下連雲服務器都要實名認證）。

總之購買好雲服務器後新增好實例，通常我是**建議使用 CentOS7 作為雲服務器的操作系統**，進入雲服務器的控制台，你會看到**服務器的公網 IP、私網 IP 和密碼**，密碼可以重新修改。

**這裡我們假設我們服務器的公網 IP 是 `1.2.3.4`。**

## 部署項目前的工作

### 安裝環境

在配置之前我們需要**將我們的 JDK、Python、MySQL、Nginx 在服務器端配置完畢**。

#### JDK

**注意：服務器安裝的 JDK 環境必須跟你的打包時使用的 JDK 版本一致，不然在運行後端 JAR 包時會報版本錯誤及不兼容的問題，要選擇 JDK1.8 以上的版本。**

這裡是建議自己在本地下載好 JDK 然後 SCP 上傳至服務器端，選擇 **Linux x64 Compressed Archive** 下載，**不要下錯成 Linux ARM 架構**的（因為我下錯過，沒仔細看名稱），壓縮包名大致為 `jdk-8u271-linux-x64.tar.gz`。

> 在用 SCP 上傳前要先進入服務器新增好資料夾，如果 SCP 上傳時指定到不存在的資料夾時，不會自動幫你新建一個新的資料夾。

```zsh
ssh root@1.2.3.4 # 使用 ssh 連上服務器

cd /home # 進入家目錄
mkdir jdk # 新建 jdk 資料夾
exit # 退出 ssh

# scp 上傳
scp ./jdk-8u271-linux-x64.tar.gz root@1.2.3.4:/home/jdk/jdk1.8.tar.gz
ssh root@1.2.3.4 # 使用 ssh 連上服務器

cd /home/jdk
tar -zxvf jdk1.8.tar.gz # 解壓縮
```

接著你就會看到目錄新增一個 JDK 目錄。下一步我們編輯環境變量：

```zsh
vi /etc/profile
```

加入以下內容：

> 自行修改目錄名，保持目錄名一致正確。

```sh
# jdk
export JAVA_HOME=/home/jdk/jdk1.8.0_271
export JAVA_JRE=/home/jdk/jdk1.8.0_271/jre
```

```zsh
source /etc/profile
java -version # 查看版本
```
#### Python

有些服務器默認只有 Python2，但是我們項目需要用到的是 Python3，我在這裡說一下怎麼讓 CentOS7 系統默認使用 Python3，不管系統是 Ubuntu 還是 CentOS 步驟都一樣，只差在安裝 Python3 的方法，

```zsh
python -V # 先查看 Python 版本
ls -al /usr/bin/python # 查看指向
yum update -y # 先更新環境
yum install python3 # 安裝 python3
python3 # 輸入 python3 驗證已經安裝好

# 會輸出以下信息表示已經安裝 python3 成功
Python 3.6.8 (default, Nov 16 2020, 16:55:22) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

接下來將系統預設的 Python2.7 改成 Python3.6。

```zsh
# 重新命名備份
mv /usr/bin/python /usr/bin/python2.7.5
# 編輯 yum 避免連接指向錯誤
vi /usr/bin/yum
```

把頭部的

```sh
#!/usr/bin/python
```

改成

```sh
#!/usr/bin/python2.7.5
```

保存退出。然後重新建立新的連結：

```zsh
rm -rf /usr/bin/python
rm -rf /usr/bin/py
# 複製並重新命名
mv /usr/bin/python3.6 /usr/bin/python
```

接下來如果輸入 `python` 就會看到預設的 Python 是 3.6.8 版本。

#### MySQL

我比較偏好使用 MariaDB，這是 MySQL 的加強版。主要是 `root` 的密碼可以是空白。

```zsh
# Installing MariaDB Server 10.4
sudo yum install wget
wget https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
chmod +x mariadb_repo_setup
sudo ./mariadb_repo_setup
sudo yum install MariaDB-server
sudo systemctl start mariadb.service
sudo mysql_secure_installation # 會要求你設置 root 密碼
```

在數據庫方面，我們經常遇到的錯誤是會**使用 `root` 去登入數據庫，常常碰到登不進去的問題**，怎麼修改 `root` 密碼也沒辦法。

這裡有個很好的解決方法，就是**新增一個一樣擁有 `root` 權限的使用者，並使用該使用者登入數據庫**就沒問題了。所以我每次都是使用這方法解決，推薦。

進入 MySQL：

```zsh
mysql
```

新增一個使用者為 `admin`，密碼為 `password`，並擁有所有權限：

> 可自行修改，不需要跟我一模一樣。

```sql
MariaDB [(none)]> GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

刷新權限：

```sql
MariaDB [(none)]>FLUSH PRIVILEGES;
```

退出：

```sql
MariaDB [(none)]>exit;
```

#### Nginx

安裝 Nginx：

```zsh
yum install nginx
```

### 新增項目目錄

我們預先將要部署的項目目錄新建好：

```zsh
cd /home
mkdir sxblog
cd sxblog
mkdir java python sql vue
```

### 開端口

因為服務器會有防火牆這些功能，要到服務器控制台的安全組將進方向和出方向需要用到的後端端口與前端端口都打開，不然外部訪問不了服務器，阿里雲的服務器有點奇妙的是無法一次將所有的端口打開，騰訊雲卻可以。

## 配置數據庫

將你項目使用的數據庫導出 `*.sql` 文件，並使用 SCP 上傳至雲服務器上。

> 修改一下自己要上傳的數據庫文件名稱，這裡假設我的數據庫文件是 `sxblog.sql`

```zsh
# 用 root 登入服務器端
scp ./sxblog.sql root@1.2.3.4:/home/sxblog/sql/sxblog.sql
```

接著將該數據庫文件導入數據庫系統：

```zsh
# 連上服務器
ssh root@1.2.3.4
# 使用文章上面我新增的使用者
mysql -u admin -p
```

```sql
MariaDB [(none)]>create database sxblog;
MariaDB [(none)]>use sxblog;
MariaDB [(sxblog)]>source /home/sxblog/sql/sxblog.sql;
```

## 將後端部署

### Spring Boot打包

#### 修改配置文件

再打包後端 SpringBoot 項目前，你需要先修改自己項目的配制文件。在 SpringBoot 項目目錄中，要**確認依賴是否是固定的版本**，然後確認是否有加入 Maven 依賴：

> 使用 JAR 包啟動 Web 後端應用並不需要在服務器上配置 Tomcat 容器環境，因為 SpringBoot 有內置 Tomcat 運行，但是要**確保你的 JDK 版本是 1.8 以上**。

```xml
<!--將應用打包成可以運行的 JAR 包-->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin<artifactId>
        </plugin>
    </plugins>
</build>
```

然後調整項目的配置參數：將數據庫的連接使用者改成是服務器上數據庫的使用者 `admin`，密碼也要變，預設的後端端口只要不跟其他後端衝突就不用改。

在**有 `pom.xml` 的目錄**下執行 Maven 的指令：

```zsh
maven clean # 清理
maven package # 打包
```

接下來就會看到目錄下出現了一個 **`target` 資料夾**，裡面是打包成 Jar 包的目錄，進入該目錄並在本地運行試試：

```zsh
# 這裡的 xxxx 需要改成你實際打包出來的包名
java -jar xxxx.jar
```

進入瀏覽器輸入運行的 IP 和 Port 就知道有沒有問題，如果運行過程有報錯，可能是打包失敗，需要查看自己的源碼哪裡寫錯了。

#### 部署到服務器

確認都沒問題了之後我們就可以部署到服務器上了。

將整個 `target` 資料夾 SCP 上傳到服務器端。

```zsh
scp -r ./target root@1.2.3.4:/home/sxblog/java/target
```

進入服務器端運行 Jar 包，**使用 `nohup` 讓該進程在我們退出連結後依然能在後台運行，`>temp.txt` 是可以指定我們的日誌輸出文件**。

```zsh
ssh root@1.2.3.4
# 使用 nohup
nohup java -jar test.jar >temp.txt &
```

### Flask

## 前端打包

在前端的項目目錄執行 `npm run build` 就可以將整個項目打包成靜態文件，並存於項目目錄的 `./dist` 資料夾。

```zsh
npm run build # 前端打包指令
```

## Reference

- [SpringBoot 项目部署到服务器的两种方式 - CSDN](https://blog.csdn.net/CSDN2497242041/article/details/106911182)
- [Linux Centos7 升级python2至python3 - 知乎](https://zhuanlan.zhihu.com/p/33660059)
- [How to Install Python 3 on CentOS 7](https://www.liquidweb.com/kb/how-to-install-python-3-on-centos-7/)
