---
title: "1 安裝DockerRegistryharbor"
description: ""
---
# 1 安裝DockerRegistryharbor

還沒有安裝Docker? 請參照這篇[[[安裝Docker]]](app://obsidian.md/%E5%AE%89%E8%A3%9DDocker)

## 1.1 簡介
學習如何安裝harbor

## 1.2 目錄

- [1 安裝DockerRegistryharbor](#1-安裝dockerregistryharbor)
  - [1.1 簡介](#11-簡介)
  - [1.2 目錄](#12-目錄)
  - [1.3 安裝Docker Registry harbor](#13-安裝docker-registry-harbor)
    - [1.3.1  進入github，尋找最新版本的harbor](#131--進入github尋找最新版本的harbor)
    - [1.3.2 將壓縮檔從github下載回來](#132-將壓縮檔從github下載回來)
    - [1.3.3 解壓縮到根目錄下](#133-解壓縮到根目錄下)
    - [1.3.4 進入到harbor目錄下](#134-進入到harbor目錄下)
    - [1.3.5  設定參數如下](#135--設定參數如下)
    - [1.3.6 將yml更名為harbor.yml](#136-將yml更名為harboryml)
    - [1.3.7 執行腳本](#137-執行腳本)
    - [1.3.8 確認容器已啟動，由此圖可得知主機對應的ports為30443](#138-確認容器已啟動由此圖可得知主機對應的ports為30443)
    - [1.3.9 看到此圖表示已安裝完成 https://markweb.idv.tw:30443/](#139-看到此圖表示已安裝完成-httpsmarkwebidvtw30443)
    - [1.3.10 備份](#1310-備份)
    - [1.3.11 查看所有上傳的image](#1311-查看所有上傳的image)
  - [1.4 同場加映:如何上傳image到私有儲存庫](#14-同場加映如何上傳image到私有儲存庫)
    - [1.4.1 請先使用docker login登入](#141-請先使用docker-login登入)
    - [1.4.2 查詢主機目前的GPG金鑰](#142-查詢主機目前的gpg金鑰)
    - [1.4.3 將金鑰初始化](#143-將金鑰初始化)
    - [1.4.4 將金鑰寫入主機，輸入兩次密碼](#144-將金鑰寫入主機輸入兩次密碼)
    - [1.4.5 輸入密碼鎖金鑰](#145-輸入密碼鎖金鑰)
    - [1.4.6 此畫面表示已成功初始化](#146-此畫面表示已成功初始化)
    - [1.4.7 再登入一次](#147-再登入一次)
  - [1.5 同場加映2:如何將harbor設定為自動啟動](#15-同場加映2如何將harbor設定為自動啟動)
    - [1.5.1 編輯以下文件](#151-編輯以下文件)
    - [1.5.2 開啟此功能並執行，輸入以下指令](#152-開啟此功能並執行輸入以下指令)



## 1.3 安裝Docker Registry harbor


### 1.3.1  進入github，尋找最新版本的harbor

https://github.com/goharbor/harbor/releases

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/Zrl215f503d-upload-30fc8229cf11cb9a805055723d9f3931.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/Zrl215f503d-upload-30fc8229cf11cb9a805055723d9f3931.png)

* * *
<!--more-->

### 1.3.2 將壓縮檔從github下載回來

```bash 
 wget https://github.com/goharbor/harbor/releases/download/v2.8.2/harbor-offline-installer-v2.8.2.tgz
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/yLCe711ba54-upload-e228d5618b216a464dc99822844fa424.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/yLCe711ba54-upload-e228d5618b216a464dc99822844fa424.png)

* * *

### 1.3.3 解壓縮到根目錄下

```bash
 tar zxf harbor-offline-installer-v2.8.2.tgz
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/ObD9333b109-upload-60d15f4e23a6732087fafebf42a49189.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/ObD9333b109-upload-60d15f4e23a6732087fafebf42a49189.png)

* * *

### 1.3.4 進入到harbor目錄下

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/Advb898f8c1-upload-7ff5e58fb8c5c807089b95710cd035cf.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/Advb898f8c1-upload-7ff5e58fb8c5c807089b95710cd035cf.png)

> [!note] 小提示 
> 1. harbor.yml 就是harbor的配置文件

### 1.3.5  設定參數如下

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/Vy9baf1b952-upload-1311822ae22e8b94ad243cfbf0eb08ae.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/Vy9baf1b952-upload-1311822ae22e8b94ad243cfbf0eb08ae.png)

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/AFlb8bb6530-upload-4b5710d4440504bef1dd1a5161446939.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/AFlb8bb6530-upload-4b5710d4440504bef1dd1a5161446939.png)

> [!note] 小提示 
> 1. hostname: 主機網域名稱
> 2. http 的 port
> 3. https 的 ports
> 4. certificate、private_key: 憑證
> 5. harbor\_admin\_password: 登入後台的密碼
> 6. password: 登入資料庫的預設密碼
>7.  data:此位置維持預設不要更動，會造成資料庫無法正常執行

### 1.3.6 將yml更名為harbor.yml

```bash
mv harbor.yml.tmpl harbor.yml
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/8Fda8db6657-upload-9b7fe9667c07e46146d71eab16ba9f03.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/8Fda8db6657-upload-9b7fe9667c07e46146d71eab16ba9f03.png)

* * *

### 1.3.7 執行腳本

```bash
.install.sh
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/xO5edd3abc5-upload-2e59cd7cfe88ec432239de9bfa330c7f.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/xO5edd3abc5-upload-2e59cd7cfe88ec432239de9bfa330c7f.png)

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/DdTb9b6cff6-upload-01a53ca75f9b06a1cfb07c311d3ac4fc.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/DdTb9b6cff6-upload-01a53ca75f9b06a1cfb07c311d3ac4fc.png)

* * *

### 1.3.8 確認容器已啟動，由此圖可得知主機對應的ports為30443

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/G5T49767e04-upload-ecfd8e76f7dd795d09de6ac92b244f09.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/G5T49767e04-upload-ecfd8e76f7dd795d09de6ac92b244f09.png)

* * *

### 1.3.9 看到此圖表示已安裝完成 https://markweb.idv.tw:30443/

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/7U16ac5be8e-upload-b16b3c4e39d0202665fe8cb496e8d040.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/7U16ac5be8e-upload-b16b3c4e39d0202665fe8cb496e8d040.png)


> [!note] 小提示 
 > 1.  預設帳號:admin
> 2.  預設密碼:admin


### 1.3.10 備份

預設路徑：/data/registry

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/wa51c3a0745-upload-57e73a1ab948897ed54517bed4716014.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/wa51c3a0745-upload-57e73a1ab948897ed54517bed4716014.png)

### 1.3.11 查看所有上傳的image

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/GfPa2bd9d5a-upload-a4d76d68083a4949282373e816c1ab99.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/GfPa2bd9d5a-upload-a4d76d68083a4949282373e816c1ab99.png)

* * *

## 1.4 同場加映:如何上傳image到私有儲存庫

### 1.4.1 請先使用docker login登入

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/1aH52644e32-upload-482bf7d4757698654a02348766cea826.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/1aH52644e32-upload-482bf7d4757698654a02348766cea826.png)

> [!error] Error saving credentials: error storing credentials - err: exit status 1,
>out: `pass store is uninitialized` 
 > 請參考下方解決方式

解決方式:

### 1.4.2 查詢主機目前的GPG金鑰

```bash
gpg --list-keys
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/qhK163cd416-upload-9126b6065fcd72235b79184a8697cc1f.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/qhK163cd416-upload-9126b6065fcd72235b79184a8697cc1f.png)

### 1.4.3 將金鑰初始化

```bash
pass init E545D9D6EC315115C2BE84DF5923EA1B8711F2AD
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/HxE81e01629-upload-675e45e01546e45dc29a10f9e4debb66.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/HxE81e01629-upload-675e45e01546e45dc29a10f9e4debb66.png)

### 1.4.4 將金鑰寫入主機，輸入兩次密碼

```bash
pass insert docker-credential-helpers/docker-pass-initialized-check
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/lXmf248aa12-upload-863c882f283925dac8a52ff5c0cbe72f.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/lXmf248aa12-upload-863c882f283925dac8a52ff5c0cbe72f.png)

### 1.4.5 輸入密碼鎖金鑰

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/gvu174b0a67-upload-bb819175f967357c3deb461b630a48a6.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/gvu174b0a67-upload-bb819175f967357c3deb461b630a48a6.png)

### 1.4.6 此畫面表示已成功初始化

```bash
pass show docker-credential-helpers/docker-pass-initialized-check
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/tSrd9bb9c59-upload-0f7ad88a8c02b9993d2fe22ca13f7ef1.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/tSrd9bb9c59-upload-0f7ad88a8c02b9993d2fe22ca13f7ef1.png)

### 1.4.7 再登入一次

```docker
docker login -u admin -p admin markweb.idv.tw:29443
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/N4n0195f8c5-upload-73b9e08abf2cf5feccd1b6539f0bcfbf.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/N4n0195f8c5-upload-73b9e08abf2cf5feccd1b6539f0bcfbf.png)

* * *

## 1.5 同場加映2:如何將harbor設定為自動啟動

### 1.5.1 編輯以下文件


```bash title:/usr/lib/systemd/system/harbor.service
vim /usr/lib/systemd/system/harbor.service
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/4Km2539d73b-upload-0b7a7f1a537c59141880abc060d1455e.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/4Km2539d73b-upload-0b7a7f1a537c59141880abc060d1455e.png)


> [!warning] 小提示
> 重點為以下這兩段，路徑一定要設對 
> 1. ExecStart=/usr/local/bin/docker-compose -f /media/markhsu/Data3/DockerImage/harbor/harbor/docker-compose.yml up
> 2. ExecStop=/usr/local/bin/docker-compose -f /media/markhsu/Data3/DockerImage/harbor/harbor/docker-compose.yml down 
> 3. 設定前請先下 which docker-compose查看docker-compose安裝路徑，有可能會變動
> 4. 請確認當前目錄下的docker-compose.yml，每個區塊是否都配置了restart：always

```bash
[Unit]
Description=Harbor
After=docker.service systemd-networkd.service systemd-resolved.service
Requires=docker.service
Documentation=http://github.com/vmware/harbor

[Service]
Type=simple
Restart=on-failure
RestartSec=5
ExecStart=/usr/local/bin/docker-compose -f /media/markhsu/Data3/DockerImage/harbor/harbor/docker-compose.yml up
ExecStop=/usr/local/bin/docker-compose -f /media/markhsu/Data3/DockerImage/harbor/harbor/docker-compose.yml down

[Install]
WantedBy=multi-user.target
```

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/560bfc3dd1e-upload-c99573a79a090ee77728467d5115f29b.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/560bfc3dd1e-upload-c99573a79a090ee77728467d5115f29b.png)

### 1.5.2 開啟此功能並執行，輸入以下指令

```bash
systemctl enable harbor
systemctl start harbor
```

