---
keywords: 實作，實作， at.js， adobe experience platform web sdk， aep web sdk
description: 瞭解如何實作 [!DNL Adobe Target] 對於使用者端Web，使用 [!DNL Adobe Experience Platform Web SDK] (AEP Web SDK)或at.js JavaScript資料庫。
title: 如何實作 [!DNL Target] 適用於使用者端Web
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 30%

---

# 概覽：實作 [!DNL Target] 適用於使用者端Web

在 [!DNL Adobe Target] 的用戶端實作中，[!DNL Target] 會將與活動相關聯的體驗直接提供給用戶端瀏覽器。瀏覽器會決定要顯示哪個體驗，然後顯示其內容。在用戶端實作中，您可以使用 WYSIWYG 編輯器、[可視化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC) 或非視覺化介面 ([表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html))，建立您的活動和個人化體驗。

實作 [!DNL Target] 使用者端，您必須使用下列其中一個JavaScript程式庫：

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)

  此 [!UICONTROL Adobe Experience Platform Web SDK] 可讓您與中的各種服務互動 [!DNL Adobe Experience Cloud] (包括 [!DNL Target])，透過 [!UICONTROL Adobe Experience Edge Network]. 如果您選擇移轉至 [!UICONTROL Adobe Experience Platform Web SDK]，請參閱 [什麼是 [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk.md).

* [[!DNL Target] at.js JavaScript資料庫](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)

  at.js JavaScript程式庫改善Web實施的頁面載入時間、改善安全性，以及為單頁應用程式提供更好的實施選項。 如果您選擇移轉至at.js，請參閱 [At.js如何運作](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) 和 [[!DNL Adobe Target] 技能培養：開發人員聊天、將Adobe Target的mbox.js遷移到at.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true).
