---
keywords: target開發人員指南；概觀；首頁
title: Adobe Target 開發人員指南
description: 如何實作和管理  [!DNL Adobe Target]  並使用其 API 和 SDK？
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 655cff9b-fc04-45cf-9068-5c6c32b70d79
source-git-commit: d98c7b890f7456de0676cadce5d6c70bc62d6140
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 12%

---

# [!DNL Adobe Target] 開發人員指南

**([檢視 [!DNL Target] 檔案更新](https://experienceleague.adobe.com/docs/target/using/release-notes/doc-change.html){target=_blank})**

這個 *[!DNL Adobe Target]開發人員指南* 提供以下專案的資源和指南： [!DNL Target] 開發人員，包括實施與管理的API和SDK檔案 [!DNL Target].

>[!NOTE]
>
>除本指南外，還有提供以下 [!DNL Adobe Target] 指南：
>
>* [*[!DNL Adobe Target] 商業從業者指南&#x200B;*](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=zh-Hant){target=_blank}
>
>* [*[!DNL Adobe Target] Tutorials *](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html){target=_blank}
>
>如需版本資訊，請參閱 [Target版本說明（最新）](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html){target=_blank} 在 *[!DNL Adobe Target]商業從業者指南*.

## 實作快速入門

**[實作之前](/help/dev/before-implement/considerations-before-you-implement-target.md)**：實作前應注意的考量事項 [!DNL Adobe Target].

## 使用者端實施

[**Adobe Experience Platform Web SDK**](/help/dev/implement/client-side/aep-web-sdk.md)：此 [!DNL Adobe Experience Platform Web SDK] 可讓您與中的各種服務互動 [!DNL Experience Cloud] (包括 [!DNL Target])，透過 [!UICONTROL AdobeExperience Edge Network].

[**Target at.js JavaScript資料庫**](/help/dev/implement/client-side/overview.md)： at.js JavaScript程式庫改善Web實施的頁面載入時間、改善安全性，以及為單頁應用程式提供更好的實施選項。

## 伺服器端實作

[**Target SDK總覽**](implement/server-side/server-side-overview.md)：開始使用 [!DNL Adobe Target] SDK，包括裝置上決策。

[**Node.js SDK**](implement/server-side/node-js/overview.md)：如何使用 [!DNL Target] Node.js SDK。

[**Java SDK**](implement/server-side/java/overview.md)：如何使用 [!DNL Target] Java SDK.

[**.NET SDK**](implement/server-side/net/overview.md)：如何使用 [!DNL Target] .NET SDK.

[**Python SDK**](implement/server-side/python/overview.md)：如何使用 [!DNL Target] Python SDK.

## 混合實施

[**混合部署**](implement/hybrid/hybrid-overview.md)：實作 [!DNL Target] 使用使用者端和伺服器端實作的組合。

## Recommendations實施

[**Recommendations實施**](implement/recommendations/recommendations.md)：計畫和實作 [!DNL Adobe Target Recommendations].

## 行動應用程式實施

[**AEP Mobile SDK總覽**](implement/mobile/overview.md)：實施方式概觀 [!DNL Adobe Target] 替換為 [!DNL Adobe Experience Platform] 行動SDK。

[**AEP Mobile SDK參考**](https://developer.adobe.com/client-sdks/documentation/)：實作 [!DNL Adobe Target] 替換為 [!DNL Adobe Experience Platform] 行動SDK。

## 電子郵件實作

[**電子郵件總覽**](implement/email/overview.md)：實施方式概觀 [!DNL Adobe Target] 在電子郵件中。

## API指南

[**簡介**](before-administer/target-api-overview.md)：概觀 [!DNL Adobe Target] API。

[**[!DNL Target Delivery API]**](/help/dev/implement/delivery-api/overview.md)：使用 [!DNL Adobe Target] 傳送API可跨網路和行動頻道以及非瀏覽器型IoT裝置（例如連線電視、資訊站或店內數位熒幕）傳送體驗。

[**[!DNL Target Admin API]**](administer/admin-api/admin-api-overview-new.md)：使用 [!DNL Adobe Target] 管理API以管理屬性、活動、受眾、選件、屬性、報表、mbox、主機、環境等。

[**[!DNL Target Profile API]**](https://developers.adobetarget.com/api/#profiles)：擷取 [!DNL Adobe Target] 使用者設定檔資訊。

[**[!DNL Target Reporting API]**](https://developer.adobe.com/target/administer/admin-api/#tag/Reports)：擷取 [!UICONTROL A/B測試] 和 [!UICONTROL Automated Personalization] 活動報表資料。

[**[!DNL Target Recommendations API]**](https://developer.adobe.com/target/administer/recommendations-api/)：使用 [!DNL Target Recommendations] API。

[**[!DNL Target Models API]**](administer/models-api/models-api-overview.md)：管理封鎖清單以定義中使用的功能 [!DNL Target] 機器學習模型。

[**ADMIN CONSOLEAPI**](https://developer.adobe.com/umapi/)：透過「Adobe使用者管理」和「使用者同步API」 ，管理使用者和產品權益。

[**[!DNL Adobe Experience Platform Edge Network Server API]**](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/overview.html)：使用 [!DNL Adobe Experience Platform Edge Network Server] 適用於各種資料收集、個人化、廣告和行銷使用案例的API。

## 資源

* [Adobe開放原始碼存放庫](https://github.com/adobe)
* [目標節點JS SDK來源](https://github.com/adobe/target-nodejs-sdk)
* [目標節點JS SDK範例存放庫](https://github.com/adobe/target-nodejs-sdk-samples)
* [目標Java SDK來源](https://github.com/adobe/target-java-sdk)
* [Target Java SDK範例存放庫](https://github.com/adobe/target-java-sdk-samples)
* [Target實作](./before-implement/prepare-to-implement-target.md)
* [Target管理](./before-administer/target-api-overview.md)
* [Adobe Target開發檔案GitHub存放庫](https://github.com/AdobeDocs/target-developers)
* [Adobe Target發行說明](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html)
* [Adobe Target商業使用手冊](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=zh-Hant)

