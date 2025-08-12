---
title: Experience Platform Web SDK中A4T資料的伺服器端記錄
description: 瞭解如何使用Experience Platform Web SDK為Adobe Analytics for Target (A4T)啟用伺服器端記錄。
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t；target；web；sdk；平台；記錄；
feature: Implementation
source-git-commit: b1b0424bfe61fb8b4e88723e6bb2c565d75f8351
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 1%

---

# [!DNL Experience Platform Web SDK]中A4T資料的伺服器端記錄

[!DNL Adobe Experience Platform Web SDK]可讓您在[!UICONTROL Adobe Analytics for Target]上實作[!UICONTROL Experience Platform Edge Network] (A4T)功能。 啟用伺服器端記錄時，所有透過Edge Network傳送的[!DNL Analytics]點選都會在伺服器端以[!DNL Target]詳細資料增加，不必經過點選拼接程式。

在資料流設定中啟用[!DNL Analytics]時，[!DNL Analytics]的伺服器端記錄已啟用：

![已啟用Analytics資料流設定](/help/dev/implement/a4t/assets/enable-analytics-datastream.png)

下圖顯示啟用伺服器端[!DNL Analytics]記錄時，資料如何流經系統：

![伺服器端記錄流程](/help/dev/implement/a4t/assets/analytics-server-side-logging.png)

## 下一步

本指南涵蓋Web SDK中A4T資料的伺服器端記錄。 請參閱[使用者端記錄](/help/dev/implement/a4t/client-side-logging.md)的指南，以取得如何在使用者端處理A4T資料的詳細資訊。