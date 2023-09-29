---
title: 使用at.js的Recommendations實作模式
description: 瞭解如何搭配at.js使用Recommendations的實作模式
feature: APIs/SDKs
level: Experienced
role: Developer
source-git-commit: 723bb2f33a011995757009193ee9c48757ae1213
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# [!DNL Recommendations] 使用at.js的實作模式概覽

此實施模式可協助您瞭解並建立您的 [!DNL Adobe Target Recommendations] 使用時的實作 [at.js JavaScript資料庫](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md).

按一下影像可展開至全熒幕。

![Adobe Target架構圖](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

請注意，影像中的數字並不代表作業順序：

1. 適用於的使用者端SDK [!DNL Adobe Target] 和 [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] 呼叫
1. [!UICONTROL EXPERIENCE CLOUDID] (ECID)贏取呼叫
1. 大量設定檔更新API和 [!DNL Customer Attributes] (CA)服務
1. 設定檔資料擷取自客戶的資料來源至 [!DNL Target] 設定檔存放區
1. 收集設定檔和行為資料，並決定要向訪客顯示的體驗
1. 體驗在頁面上呈現
1. at.js會呈現頁面上的體驗

每個模式都包含不同零件，每個零件都對應至您的關鍵實作需求 [!DNL Target] 實作。

本指南中的個別文章會詳細說明每個部分：

* [初始化SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [設定資料彙集](/help/dev/patterns/recs-atjs/data-collection.md)
* [演算體驗](/help/dev/patterns/recs-atjs/render-experiences.md)
* [通知Target](/help/dev/patterns/recs-atjs/notify-target.md)

