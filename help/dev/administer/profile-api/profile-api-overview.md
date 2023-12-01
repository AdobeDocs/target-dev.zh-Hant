---
title: Adobe Target設定檔API概述
description: 瞭解如何使用Adobe Target設定檔API將訪客資料傳送至 [!DNL Target].
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: af9db32d59bdf32f2b9fade267922803250377dd
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# [!DNL Adobe Target Profile APIs overview]

使用者個人資料包含網頁訪客的人口統計和行為資訊，例如年齡、性別、購買的產品、上次造訪時間等。 [!DNL Adobe Target] 使用此資訊為每位訪客提供個人化內容。

每位訪客的設定檔資訊會儲存在Cookie或協力廠商應用程式中。

如果您的網頁實作Target程式碼([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) 或 [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md))，則來自Cookie的設定檔資訊會傳遞至 [!DNL Target] 使用設定檔引數。 [!DNL Target] 會透過 `pcID` 會產生訪客的Cookie。 不過，您可以使用透過mbox呼叫從外部應用程式傳遞設定檔引數 `mbox3rdPartyIds`.

使用 [!DNL Adobe Target] 當您有訪客的設定檔資料可傳送至時，可使用設定檔API [!DNL Target] 或不想要當作頁面式整合的一部分傳送的電子郵件對象 [!DNL Target]. 這可能是來自客戶關係管理(CRM)或銷售點(POS)系統的資料，在頁面上無法使用，或是較敏感性質的資料，在頁面上傳遞沒有意義。

透過API更新設定檔有兩種方式：

* [單一設定檔更新 API](/help/dev/administer/profile-api/profile-single-api.md)
* [透過批次大量更新設定檔](/help/dev/administer/profile-api/profile-bulk-api.md)
