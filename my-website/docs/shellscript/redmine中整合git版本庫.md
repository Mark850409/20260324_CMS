---
title: "1 redmine中整合git版本庫"
description: ""
---
# 1 redmine中整合git版本庫

## 1.1 簡介
學習如何透過redmine中整合git版本庫

## 1.2 目錄

- [1 redmine中整合git版本庫](#1-redmine中整合git版本庫)
  - [1.1 簡介](#11-簡介)
  - [1.2 目錄](#12-目錄)
    - [1.2.1 事前準備](#121-事前準備)
    - [1.2.2 設定redmine儲存機制\&建立關鍵字進行事件觸發](#122-設定redmine儲存機制建立關鍵字進行事件觸發)
    - [1.2.3 透過Jenkins進行事件觸發\&撰寫自動更新腳本](#123-透過jenkins進行事件觸發撰寫自動更新腳本)

### 1.2.1 事前準備

請先建立好gitea倉庫

[![image-20230917201642307](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/G1U47501508-image-20230917201642307.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/G1U47501508-image-20230917201642307.png)

在redmine開 個新issue

[![image-20230917202010429](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/yNn302d5927-image-20230917202010429.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/yNn302d5927-image-20230917202010429.png)

<!--more-->

### 1.2.2 設定redmine儲存機制&建立關鍵字進行事件觸發

設定→儲存機制→建立新儲存機制
[![image-20230917202220585](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/a6X1a477a34-image-20230917202220585.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/a6X1a477a34-image-20230917202220585.png)


按照圖片進行相關設定
[![image-20230917202515300](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/TwC1dd40f53-image-20230917202515300.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/TwC1dd40f53-image-20230917202515300.png)

### 1.2.3 透過Jenkins進行事件觸發&撰寫自動更新腳本

設定GIT_URL
[![image-20230917202711770](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/W8me6a7d08d-image-20230917202711770.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/W8me6a7d08d-image-20230917202711770.png)


設定PROJECT
[![image-20230917202800333](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/ZeE6efa47df-image-20230917202800333.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/ZeE6efa47df-image-20230917202800333.png)


綁定GIT
[![image-20230917202848539](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/QBYe381803a-image-20230917202848539.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/QBYe381803a-image-20230917202848539.png)


設定每分鐘觸發 次事件
[![image-20230917203014533](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/85I28d5974f-image-20230917203014533.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/85I28d5974f-image-20230917203014533.png)

撰寫腳本

```bash
#!/bin/bash
#+-------------------------------------腳本說明--------------------------------------------+
# Redmine自動更新腳本
# 使用方式: ./autoSyncRedminegit.sh
#
# (C) 2023 - markhsu - licensed under markweb License v1.
#
#+----------------------------------------------------------------------------------------+
#                                    每分鐘同步一次 repos, 依照需求自行增加
#+----------------------------------------------------------------------------------------+
docker exec redmine bash -c "if [ -d "/data/redmine/repos/${PROJECT}" ]; then cd /data/redmine/repos/'${PROJECT}' && git fetch --all; else git clone --mirror ${GIT_URL} && cd /data/redmine/repos/'${PROJECT}' && git fetch --all ; fi"
```

[![image-20230917203054726](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/QJu05294aa7-image-20230917203054726.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/QJu05294aa7-image-20230917203054726.png)

建置後不論成功或失敗都要寄信

[![image-20230917203146001](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/6S985984174-image-20230917203146001.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/6S985984174-image-20230917203146001.png)


以下為成功畫面

[![image-20230917203316600](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/6k902cb413a-image-20230917203316600.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/6k902cb413a-image-20230917203316600.png)

[![image-20230917203419580](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/igD72166443-image-20230917203419580.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/igD72166443-image-20230917203419580.png)

[![image-20230917203559762](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/scaled-1680-/AlH434230c4-image-20230917203559762.png)](https://zanehsu.myqnapcloud.com:6876/uploads/images/gallery/2025-08/AlH434230c4-image-20230917203559762.png)
