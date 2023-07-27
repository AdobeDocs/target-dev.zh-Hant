---
keywords: 實作, at.js, javascript 程式庫
description: 瞭解如何部署 [!DNL Adobe Target]  at.js JavaScript資料庫使用中的標籤 [!DNL Adobe Experience Platform] 或沒有標籤管理員。
title: 如何部署at.js？
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 15%

---

# 如何部署 at.js

關於如何部署的資訊 [!DNL Adobe Target]  JavaScript程式庫at.js，使用中的標籤 [!DNL Adobe Experience Platform] 或沒有標籤管理員。

您可使用下列方法部署 at.js:

* **[實作 [!DNL Target] 使用Adobe Experience Platform中的標籤](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**：中的標籤 [!DNL Adobe Experience Platform] 是Adobe推出的新一代標籤管理功能。 標籤可讓客戶透過簡單的方式部署及管理必要的分析、行銷及廣告標籤功能，以便支援相關客戶體驗。

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] 已經過品牌重塑，現在是 [!DNL Adobe Experience Platform] 中的一套資料彙集技術。 因此，所有產品文件中出現了幾項術語變更。請參閱下列內容 [檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 以取得術語變更的彙整參考資料。

* **[實作 [!DNL Target] 不使用Tag Manager](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**：您可以實作 [!DNL Target] 不使用標籤管理程式(例如 [!DNL Adobe Experience Platform])。
* **實作 [!DNL Target] 使用協力廠商標籤管理員**： [Adobe Experience Platform中的標籤](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 是實施的偏好方法 [!DNL Target]；不過，您也可以實作 [!DNL Target] 使用協力廠商標籤管理程式，包括Tealium、Ensighten和Google Tag。 如需使用Launch的好處清單，請參閱 [使用實作at.js的優點 [!DNL Adobe Target]  副檔名](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension).

  不過，如果您知道如何實作 [!DNL Target] 若不使用標籤管理員，您可以使用協力廠商標籤管理員輕鬆實作，而不必在網站程式碼中硬式編碼at.js。

  以下是兩個可協助您實作的相關主題 [!DNL Target] 使用協力廠商標籤管理程式：

   * [實作之前](/help/dev/before-implement/prepare-to-implement-target.md)
   * [實作 [!DNL Target] 不使用標籤管理程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  請務必檢視協力廠商標籤管理員的檔案以取得詳細資訊。

實作 [!DNL Target] 使用單頁應用程式(SPA)時，請參閱 [實作單頁應用程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).
