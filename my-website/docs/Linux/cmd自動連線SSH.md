---
title: "1 cmd自動連線SSH"
description: ""
---
# 1 cmd自動連線SSH

## 1.1 簡介
學習如何利用cmd自動連線LINUX SSH

## 1.2 目錄

- [1 cmd自動連線SSH](#1-cmd自動連線ssh)
  - [1.1 簡介](#11-簡介)
  - [1.2 目錄](#12-目錄)
    - [1.2.1 開啟windows命令提示字元](#121-開啟windows命令提示字元)
    - [1.2.2 下拉選單選擇設定](#122-下拉選單選擇設定)
    - [1.2.3 開啟setting.json，在actions底下輸入指令](#123-開啟settingjson在actions底下輸入指令)
    - [1.2.4 將setting.json存檔後，直接開啟測試，看到此畫面表示已完成](#124-將settingjson存檔後直接開啟測試看到此畫面表示已完成)


### 1.2.1 開啟windows命令提示字元

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/pXUceb51164-upload-56c74670c50f687b6ee6d5329f1cad06.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/pXUceb51164-upload-56c74670c50f687b6ee6d5329f1cad06.png)

* * *
<!--more-->

### 1.2.2 下拉選單選擇設定

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/Vxla33a23bb-upload-8bc2cff44294ba3a8fe4ba18f87ade83.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/Vxla33a23bb-upload-8bc2cff44294ba3a8fe4ba18f87ade83.png)

* * *

### 1.2.3 開啟setting.json，在actions底下輸入指令

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/9TT746ff5ff-upload-19351111c6808a491617ff0cc560d856.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/9TT746ff5ff-upload-19351111c6808a491617ff0cc560d856.png)

```
{
 "commandline": "ssh root@192.168.0.175",
 "elevate": false,
 "guid": "{a54fc4f8-79dd-52b9-8b33-483463c3950d}",
 "name": "markwebSSH"
}
```

* * *

### 1.2.4 將setting.json存檔後，直接開啟測試，看到此畫面表示已完成

[![image](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/oVL57b7c4bc-upload-d6087f4ee9cbcbc3f3da075da04dee4a.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/oVL57b7c4bc-upload-d6087f4ee9cbcbc3f3da075da04dee4a.png)
