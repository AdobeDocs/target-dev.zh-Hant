---
keywords: 實作, at.js, javascript 程式庫
description: 瞭解如何使用 [!DNL Adobe Experience Platform] 中的標籤或不使用標籤管理員來部署 [!DNL Adobe Target]  at.js JavaScript資料庫。
title: 如何部署at.js？
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 10%

---

# 如何部署 at.js

有關如何使用[!DNL Adobe Experience Platform]中的標籤或不使用標籤管理員來部署[!DNL Adobe Target] JavaScript資料庫at.js的資訊。

您可使用下列方法部署 at.js:

* 使用Adobe Experience Platform ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**中的標籤**[&#x200B;實作 [!DNL Target] ： [!DNL Adobe Experience Platform]中的標籤是新一代Adobe標籤管理功能。 標籤可讓客戶透過簡單的方式部署及管理必要的分析、行銷及廣告標籤功能，以便支援相關客戶體驗。

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] 已經過品牌重塑，現在是 [!DNL Adobe Experience Platform] 中的一套資料彙集技術。 因此，所有產品檔案中出現了幾項術語變更。 請參閱下列[檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html)，以取得術語變更的彙總參考資料。

* **[實作 [!DNL Target] 不使用標籤管理員](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**：您可以實作[!DNL Target]而不使用標籤管理員（例如，[!DNL Adobe Experience Platform]中的標籤）。
* **使用協力廠商標籤管理員實作[!DNL Target]**： Adobe Experience Platform中的[標籤是實作[!DNL Target]的偏好方法；不過，您也可以使用協力廠商標籤管理員實作[!DNL Target]，包括Tealium、Ensighten和Google標籤。 ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)如需使用Launch的好處清單，請參閱[使用 [!DNL Adobe Target] 擴充功能實作at.js的優點](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension)。

  不過，如果您知道如何在不使用標籤管理員的情況下實作[!DNL Target]，則可以使用協力廠商標籤管理員輕鬆實作，而不是在網站程式碼中硬式編碼at.js。

  以下是兩個相關主題，將協助您使用協力廠商標籤管理員實作[!DNL Target]：

   * [實作之前](/help/dev/before-implement/prepare-to-implement-target.md)
   * [實作 [!DNL Target] 而不使用標籤管理員](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  請務必檢視協力廠商標籤管理員的檔案以取得詳細資訊。

若要在使用單頁應用程式(SPA)時實作[!DNL Target]，請參閱[單頁應用程式實作](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。
