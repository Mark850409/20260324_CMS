---
title: "1 Jenkins自動化佈署-自動建置並佈署python到遠端主機"
description: ""
---
# 1 Jenkins自動化佈署-自動建置並佈署python到遠端主機

還沒有安裝Jenkins? 請參照這篇[[Jenkins自動化部署-安裝教學]]
## 1.1 簡介
學習如何利用Jenkins遠端部署python

## 1.2 目錄

- [1 Jenkins自動化佈署-自動建置並佈署python到遠端主機](#1-jenkins自動化佈署-自動建置並佈署python到遠端主機)
  - [1.1 簡介](#11-簡介)
  - [1.2 目錄](#12-目錄)
  - [1.3 建立git專案](#13-建立git專案)
  - [1.4 建立Jenkins作業流程](#14-建立jenkins作業流程)
    - [1.4.1 建立git倉庫後，將ssh的URL貼到Repository URL上面，並選擇 開始建立好的金鑰](#141-建立git倉庫後將ssh的url貼到repository-url上面並選擇-開始建立好的金鑰)
    - [1.4.2 設定專案觸發＆排程](#142-設定專案觸發排程)
    - [1.4.3 將apache站台設定檔+金鑰檔+腳本，複製到遠端主機上面](#143-將apache站台設定檔金鑰檔腳本複製到遠端主機上面)
    - [1.4.4 建置後動作選擇寄信，附檔和建置紀錄可加可不加，最後按下儲存即可](#144-建置後動作選擇寄信附檔和建置紀錄可加可不加最後按下儲存即可)
    - [1.4.5 執行結果如附圖參考](#145-執行結果如附圖參考)

## 1.3 建立git專案

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/eBa9c016163-upload-78c755dd5415f034a80c2bb67a1e4a20.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/eBa9c016163-upload-78c755dd5415f034a80c2bb67a1e4a20.png)

<!--more-->

## 1.4 建立Jenkins作業流程

### 1.4.1 建立git倉庫後，將ssh的URL貼到Repository URL上面，並選擇 開始建立好的金鑰
[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/FcJ0ae07a47-upload-70bdab29a7e8324f58e72225c955fea1.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/FcJ0ae07a47-upload-70bdab29a7e8324f58e72225c955fea1.png)

### 1.4.2 設定專案觸發＆排程
[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/D94ad3e7e42-upload-280e1126340490755ada5417e1a54324.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/D94ad3e7e42-upload-280e1126340490755ada5417e1a54324.png)

### 1.4.3 將apache站台設定檔+金鑰檔+腳本，複製到遠端主機上面
[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/j0u329f6d80-upload-2dd764a3f3c9656c188e9951cf97d10d.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/j0u329f6d80-upload-2dd764a3f3c9656c188e9951cf97d10d.png)

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/BjP6863cae9-upload-f6d8cad31b3dca5fa5993366561048a4.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/BjP6863cae9-upload-f6d8cad31b3dca5fa5993366561048a4.png)


[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/ZA5cc5608d3-upload-0023239bff2a8077d6c63ddd70340270.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/ZA5cc5608d3-upload-0023239bff2a8077d6c63ddd70340270.png)


[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/wSXa034a8d1-upload-1b663b88ef4eb6b64261325cbb2da2a8.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/wSXa034a8d1-upload-1b663b88ef4eb6b64261325cbb2da2a8.png)

### 1.4.4 建置後動作選擇寄信，附檔和建置紀錄可加可不加，最後按下儲存即可
[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/A4T0d7832ea-upload-59e4ce5c8b9316dfcd3737f5508b5500.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/A4T0d7832ea-upload-59e4ce5c8b9316dfcd3737f5508b5500.png)


### 1.4.5 執行結果如附圖參考
[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/i8H2f3f4bfc-upload-b2bd5120a24b95c6ed42da145ffc3f7f.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/i8H2f3f4bfc-upload-b2bd5120a24b95c6ed42da145ffc3f7f.png)
