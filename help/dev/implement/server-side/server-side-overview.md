---
keywords: 伺服器端，伺服器端， api， sdk， node.js， nodejs， node js， recommendations api， api， api，伺服器端1
description: 瞭解 [!DNL Adobe Target] 伺服器端傳送API、SDK和 [!DNL Target Recommendations] API。
title: 我可以在何處瞭解 [!DNL Target] 伺服器端傳送API和SDK？
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
source-git-commit: 75af30045684b95d5989b0a1f877ba95bb8cd883
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 13%

---

# 伺服器端：實作[!DNL Target]

有關[!DNL Adobe Target]伺服器端傳送API、SDK和[!DNL Target Recommendations] API的資訊。

>[!NOTE]
>
>如果您的實作在使用者端使用at.js和[!DNL AppMeasurement]，您應使用下述的[!UICONTROL Target Delivery API]和伺服器端SDK。
>
>如果您的實作使用[!UICONTROL Adobe Experience Platform Web SDK]，您應該使用[[!UICONTROL Adobe Experience Platform] [!UICONTROL Edge Network Server API]](https://experienceleague.adobe.com/zh-hant/docs/experience-platform/edge-network-server-api/overview){target=_blank}。

下列程序會發生在 [!DNL Target] 的伺服器端實作中:

1. 用戶端裝置會透過伺服器提出體驗的要求。
1. 伺服器會將該要求傳送至 [!DNL Target]。
1. [!DNL Target] 會將回應傳送回伺服器。
1. 您的伺服器會決定要將哪個體驗提供給使用者端裝置以供其呈現。

體驗不需要顯示在瀏覽器中。 體驗可以透過語音助理或某些其他非視覺體驗或非瀏覽器型裝置，顯示在電子郵件或資訊站中。 由於伺服器位於用戶端與 [!DNL Target] 之間，如果您需要更多控制和安全性，或有要在伺服器上執行的複雜後端程序，這種類型的實施也是非常理想的選擇。

>[!NOTE]
>
>首次訪客只能在使用者端上初始化。 無法在伺服器端初始化首次訪客&#x200B;**。 這是因為ECID，其取決於協力廠商Demdex Cookie，因此需要透過訪客API.js在涉及瀏覽器的實施上進行初始化。

以下章節提供有關各種伺服器端API和SDK的詳細資訊：

## 伺服器端傳送 API

連結： [伺服器端傳送API](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

透過[!DNL Target]傳遞API，您可以：

* 跨網路(包括SPA)和行動裝置頻道，以及非瀏覽器型IoT裝置（例如連線電視、資訊站或店內數位熒幕）提供體驗。
* 從任何可進行HTTP/s呼叫的伺服器端平台或應用程式提供體驗。
* 無論訪客使用哪些管道或裝置與您的業務互動，都可為訪客提供一致且個人化的體驗。
* 快取伺服器上工作階段中訪客的體驗，以避免多個API呼叫，進而提高效能。
* 從伺服器端順暢地與Adobe Experience Cloud產品(例如Adobe Analytics、Adobe Audience Manager (AAM)和Experience CloudID服務)整合。

## 伺服器端SDK

[!DNL Adobe Target]伺服器端SDK檔案可協助您在伺服器上以您選擇的語言實作[!DNL Target]。

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

透過[!DNL Adobe Target]的伺服器端SDK，您可以：

* 在&#x200B;**幾乎零延遲**&#x200B;處執行並執行&#x200B;**功能標幟**、**轉出**&#x200B;和&#x200B;**A/B實驗**。
* 透過&#x200B;**網頁**&#x200B;提供體驗，包括&#x200B;**SPA**&#x200B;和&#x200B;**行動頻道**，以及非瀏覽器型的&#x200B;**物聯網(IoT)裝置**，例如連線電視、資訊站或店內數位熒幕。
* 無論使用者與您的企業使用哪個管道或裝置，都將&#x200B;**機器學習(ML)驅動的個人化體驗**&#x200B;傳遞給使用者。
* **從伺服器端順暢地整合Adobe Experience Cloud**&#x200B;產品，例如&#x200B;**Adobe Analytics**、**Adobe Audience Manager**&#x200B;以及&#x200B;**Experience CloudID服務**。

請參閱[快速入門](sdk-guides/getting-started/getting-started.md)頁面，瞭解如何透過[裝置上決策](sdk-guides/on-device-decisioning/overview.md)執行簡單的功能標幟使用案例。

檢視我們的[範例應用程式](sdk-guides/sample-apps/sample-apps.md)，享受樂趣和玩樂！

## [!DNL Target Recommendations] API

連結： [目標Recommendations API](https://developers.adobetarget.com/api/recommendations)和[Adobe Recommendations API總覽](../../before-administer/recs-api/overview.md)。

Recommendations API可讓您以程式設計方式與[!DNL Target]推薦伺服器互動。 這些API可與一系列應用程式棧疊整合，以執行您通常透過[!DNL Target]使用者介面執行的功能。
