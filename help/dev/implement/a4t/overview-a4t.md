---
title: Adobe Analytics for Target (A4T)登入Experience Platform Web SDK
description: 瞭解如何使用Experience Platform Web SDK控制Adobe Analytics for Target (A4T)資料的收集。
seo-title: Adobe Analytics for Target (A4T) Logging in the Experience Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t；記錄；analytics；sdk；web sdk；
feature: Implementation
source-git-commit: b7638f7ab3fe9a6551c9d542a990e22ddb2b27a2
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 1%

---

# [!DNL Adobe Analytics for Target] (A4T)登入[!DNL Experience Platform Web SDK]

使用[!DNL Adobe Target]進行個人化時，您可以選擇您要使用哪個系統進行效能測量。 每個[Target活動](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=zh-Hant)可讓您在[!DNL Target]報告與Adobe [!DNL Analytics]報告之間選取。

如果您使用[!DNL Analytics]報告，[!DNL Target]必須與[!DNL Analytics]溝通下列專案：

* 您的訪客已進入哪些[!DNL Target]活動
* 他們看過的體驗
* 已達到哪個轉換

[!DNL Adobe Experience Platform Web SDK]支援兩種型別的[!DNL Analytics] (A4T)使用案例的[!UICONTROL Analytics for Target]記錄：

| 記錄方法 | 說明 |
| --- | --- |
| 伺服器端[!DNL Analytics]記錄 | 透過Edge Network傳送的所有[!DNL Analytics]點選在伺服器端都以[!DNL Target]詳細資料擴增，不必經過點選拼接程式。 |
| 使用者端[!DNL Analytics]記錄 | 使用者端傳回[!DNL Target]資料，可讓您使用[!DNL Analytics]資料插入API[手動增加資料並傳送至](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html?lang=zh-Hant)。 |

記錄方法取決於您是否已在設定的[!DNL Adobe Analytics]資料流[上啟用](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/datastreams/overview)：

![正在記錄方法決定流程](/help/dev/implement/a4t/assets/analytics-logging.png)

## 下一步

本文簡介了網頁SDK中A4T資料的各種記錄方法。 如需上述各方法的詳細資訊，請參閱下列檔案：

* [Experience Platform Web SDK中A4T資料的伺服器端記錄](/help/dev/implement/a4t/client-side-logging.md)
* [Experience Platform Web SDK中A4T資料的使用者端記錄](/help/dev/implement/a4t/client-side-logging.md)