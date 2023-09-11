---
title: 實作模式概觀
description: 瞭解如何使用實作模式
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: e15513f5c52240536ccf41f16ba7f4dc6dbf9a04
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# 實作模式概觀

[!DNL Adobe Target] 實作模式可協助您瞭解並建立您的下列架構 [!DNL Target] 實作。

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

本指南中會以個別主題說明每個部分。 例如， [[!DNL Recommendations] 使用at.js的實作模式](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md) 包含下列主題：

* 初始化SDK
* 設定資料彙集
* 演算體驗
* 通知 [!DNL Target]

