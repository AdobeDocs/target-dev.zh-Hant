---
title: 使用at.js的Recommendations實施模式
description: 瞭解如何搭配at.js使用Recommendations的實施模式
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: d568cd1d-acc3-42e0-ae2c-5787e6f361f8
TQID: https://experienceleague.adobe.com/uYNa5mL-8ffPS-ddjnQzILnPFMbjNJqKZDmQT8qFeGA
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c4147b6e-073b-4d3c-9ab1-d60f2f4434ef
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 155
ht-degree: 0%

---

# 使用at.js的[!DNL Recommendations]實作模式概覽

此實作模式可協助您在使用[at.js JavaScript資料庫](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)時瞭解並建立您的[!DNL Adobe Target Recommendations]實作。

按一下影像可展開至全熒幕。

![Adobe Target架構圖](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

請注意，影像中的數字並不代表作業順序：

1. [!DNL Adobe Target]和[!DNL Experience Cloud ID Service]的使用者端SDK
1. [!DNL Target Delivery API]呼叫
1. [!UICONTROL Experience Cloud ID] (ECID)贏取呼叫
1. 大量設定檔更新API和[!DNL Customer Attributes] (CA)服務
1. 從客戶的資料來源將設定檔資料擷取至[!DNL Target]設定檔存放區
1. 收集設定檔和行為資料，並決定要向訪客顯示的體驗
1. 體驗在頁面上呈現
1. at.js會呈現頁面上的體驗

每個模式都包含不同的部分，每個部分都對應至[!DNL Target]實作的關鍵實作需求。

本指南中的個別文章會詳細說明每個部分：

* [初始化SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [設定資料彙集](/help/dev/patterns/recs-atjs/data-collection.md)
* [演算體驗](/help/dev/patterns/recs-atjs/render-experiences.md)
* [通知Target](/help/dev/patterns/recs-atjs/notify-target.md)
