---
title: Experience Platform Web SDK中A4T資料的伺服器端記錄
description: 瞭解如何使用Experience Platform Web SDK為Adobe Analytics for Target (A4T)啟用伺服器端記錄。
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t；target；web；sdk；平台；記錄；
feature: Implementation
exl-id: 716f7343-69a6-44d7-baec-a0a0df1b6e1f
TQID: https://experienceleague.adobe.com/I7-G2VO2AN3qFsgkk4JX2Pg6WJfZq0HZkcGL4XQNoWg
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 161
ht-degree: 1%

---

# [!DNL Experience Platform Web SDK]中A4T資料的伺服器端記錄

[!DNL Adobe Experience Platform Web SDK]可讓您在[!UICONTROL Adobe Analytics Edge Network]上實作[!UICONTROL Experience Platform for Target] (A4T)功能。 啟用伺服器端記錄時，所有透過Edge Network傳送的[!DNL Analytics]點選都會在伺服器端以[!DNL Target]詳細資料增加，不必經過點選拼接程式。

在資料流設定中啟用[!DNL Analytics]時，[!DNL Analytics]的伺服器端記錄已啟用：

![已啟用Analytics資料流設定](/help/dev/implement/a4t/assets/enable-analytics-datastream.png)

下圖顯示啟用伺服器端[!DNL Analytics]記錄時，資料如何流經系統：

![伺服器端記錄流程](/help/dev/implement/a4t/assets/analytics-server-side-logging.png)

## 下一步

本指南涵蓋Web SDK中A4T資料的伺服器端記錄。 請參閱[使用者端記錄](/help/dev/implement/a4t/client-side-logging.md)的指南，以取得如何在使用者端處理A4T資料的詳細資訊。
