---
title: 更新設定檔
description: 瞭解如何使用Adobe Target設定檔API將訪客資料傳送至 [!DNL Target]。
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: 315e8fbe67938588c3c9a0135e0cd85fa1f12187
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 1%

---

# 更新設定檔

使用者個人資料包含網頁訪客的人口統計和行為資訊，例如年齡、性別、購買的產品、上次造訪時間等。 [!DNL Adobe Target]使用此資訊為每位訪客提供個人化內容。

每位訪客的設定檔資訊會儲存在Cookie或協力廠商應用程式中。

如果您的網頁實作[!DNL Target]程式碼([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)或[Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md))，則會使用設定檔引數將Cookie中的設定檔資訊傳遞至[!DNL Target]。 [!DNL Target]會透過在訪客的Cookie中產生的`pcID`唯一識別每個訪客。 不過，您可以使用`mbox3rdPartyIds`，透過mbox呼叫從外部應用程式傳遞設定檔引數。

當您有訪客的設定檔資料要傳送至[!DNL Adobe Target]時，使用[!DNL Target]設定檔API，您無法或不想傳送這些資料作為您與[!DNL Target]的頁面式整合的一部分。 這可能是來自客戶關係管理(CRM)或銷售點(POS)系統的資料，但頁面上未提供。 或者，此資料可能屬於較敏感性質，沒有必要在頁面上傳遞。

透過API更新設定檔有兩種方式：

* [單一輪廓更新 API](/help/dev/administer/profile-api/profile-single-api.md)
* [透過批次大量更新設定檔](/help/dev/administer/profile-api/profile-bulk-api.md)
