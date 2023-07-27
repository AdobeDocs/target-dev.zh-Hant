---
title: 實作模式概觀
description: 瞭解如何使用實作模式
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: c4b9dfed19e5e4a56bfeae26c4310b997a2d617e
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# 實作模式概觀

[!DNL Adobe Target] 實作模式可協助您瞭解並建立您的下列架構 [!DNL Target] 實作。

按一下影像可展開至全熒幕。

![Adobe Target架構圖](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

請注意，影像中的數字並不代表作業順序：

1. 適用於的使用者端SDK [!DNL Adobe Target] 和 [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] 呼叫
1. ECID贏取呼叫
1. 大量設定檔更新API和 [!DNL Customer Attributes] (CA)服務
1. 設定檔資料擷取自客戶的資料來源至 [!DNL Target] 設定檔存放區
1. 收集設定檔/行為資料，並決定要向一般使用者顯示的體驗
1. 體驗在頁面上呈現
1. at.js會呈現頁面上的體驗

每個陣列都由不同的零件組成。 每個部分都對應至貴機構的關鍵實施需求 [!DNL Target] 實作。

本指南的個別頁面會詳細說明每個部分。 例如， [!DNL Target] 實施模式包含下列頁面：

* 初始化SDK
* 豐富設定檔
* 演算體驗
* 通知 [!DNL Target]

