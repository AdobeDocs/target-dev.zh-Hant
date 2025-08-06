---
keywords: 實作，實作， at.js， adobe experience platform web sdk， aep web sdk
description: 瞭解如何使用 [!DNL Adobe Target] (AEP Web SDK)或at.js JavaScript資料庫為使用者端Web實作 [!DNL Adobe Experience Platform Web SDK] 。
title: '如何為使用者端Web實作 [!DNL Target] '
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: 7e2f1620c839393051432485192a45ddda2390a0
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 28%

---

# 概觀：為使用者端Web實作[!DNL Target]

在 [!DNL Adobe Target] 的用戶端實作中，[!DNL Target] 會將與活動相關聯的體驗直接提供給用戶端瀏覽器。瀏覽器會決定要顯示哪個體驗，然後顯示其內容。在用戶端實作中，您可以使用 WYSIWYG 編輯器、[可視化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=zh-Hant) (VEC) 或非視覺化介面 ([表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=zh-Hant))，建立您的活動和個人化體驗。

若要實作[!DNL Target]使用者端，您必須使用下列其中一個JavaScript資料庫：

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)

  [!UICONTROL Adobe Experience Platform Web SDK]可讓您透過[!DNL Adobe Experience Cloud]與[!DNL Target] （包括[!UICONTROL Adobe Experience Edge Network]）中的各種服務互動。 如果您選擇移轉至[!UICONTROL Adobe Experience Platform Web SDK]，請參閱[什麼是[!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)。

* [[!DNL Target] at.js JavaScript資料庫](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)

  at.js JavaScript程式庫改善Web實施的頁面載入時間、改善安全性，以及為單頁應用程式提供更好的實施選項。 如果您選擇移轉至at.js，請參閱[At.js的運作方式](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)及[[!DNL Adobe Target] 技能建立：開發人員聊天，將Adobe Target的mbox.js移轉至at.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true)。


請參閱[比較at.js程式庫與Web SDK](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md)，瞭解兩種實作方法之間的差異。