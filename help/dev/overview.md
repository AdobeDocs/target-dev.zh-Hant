---
keywords: target開發人員指南；概觀；首頁
title: Adobe Target開發人員指南
description: 如何實作和管理 [!DNL Adobe Target] ，以及如何使用其API和SDK？
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 655cff9b-fc04-45cf-9068-5c6c32b70d79
source-git-commit: dadc3804da4592dba4ad88b8c5c9f804c56e232b
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 6%

---

# [!DNL Adobe Target] 開發人員指南

**（[檢視 [!DNL Target] 檔案更新](https://experienceleague.adobe.com/docs/target/using/release-notes/doc-change.html){target=_blank}）**

此&#x200B;*[!DNL Adobe Target]開發人員指南*&#x200B;為[!DNL Target]開發人員提供資源和指南，包括實施和管理[!DNL Target]的API和SDK檔案。

>[!NOTE]
>
>除本指南外，還有提供以下 [!DNL Adobe Target] 指南：
>
>* [*[!DNL Adobe Target]商業從業者指南&#x200B;*](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=zh-Hant){target=_blank}
>
>* [*[!DNL Adobe Target]個Tutorials *](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html){target=_blank}
>
>如需發行資訊，請參閱&#x200B;*[!DNL Adobe Target]商業從業者指南*&#x200B;中的[Target發行說明（最新）](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html){target=_blank}。

## 實作快速入門

**[實作之前](/help/dev/before-implement/considerations-before-you-implement-target.md)**：實作[!DNL Adobe Target]之前應解決哪些考量事項。

## 使用者端實施

[**Adobe Experience Platform Web SDK**](/help/dev/implement/client-side/aep-web-sdk.md)： [!DNL Adobe Experience Platform Web SDK]可讓您透過[!UICONTROL Adobe Experience Edge Network]與[!DNL Experience Cloud] （包括[!DNL Target]）中的各種服務互動。

[**Target at.js JavaScript資料庫**](/help/dev/implement/client-side/overview.md)： at.js JavaScript資料庫可改善Web實施的頁面載入時間、改善安全性，以及為單頁應用程式提供更好的實施選項。

## 伺服器端實作

[**Target SDK總覽**](implement/server-side/server-side-overview.md)：開始使用[!DNL Adobe Target] SDK，包括裝置上決策。

[**Node.js SDK**](implement/server-side/node-js/overview.md)：如何使用[!DNL Target] Node.js SDK。

[**Java SDK**](implement/server-side/java/overview.md)：如何使用[!DNL Target] Java SDK。

[**.NET SDK**](implement/server-side/net/overview.md)：如何使用[!DNL Target] .NET SDK。

[**Python SDK**](implement/server-side/python/overview.md)：如何使用[!DNL Target] Python SDK。

## 混合實施

[**混合式部署**](implement/hybrid/hybrid-overview.md)：使用使用者端和伺服器端實作的組合實作[!DNL Target]。

## Recommendations實施

[**Recommendations實施**](implement/recommendations/recommendations.md)：計畫和實施[!DNL Adobe Target Recommendations]。

## 行動應用程式實施

[**AEP Mobile SDK總覽**](implement/mobile/overview.md)：如何使用[!DNL Adobe Experience Platform] Mobile SDK實作[!DNL Adobe Target]的總覽。

[**AEP Mobile SDK參考**](https://developer.adobe.com/client-sdks/documentation/)：使用[!DNL Adobe Experience Platform]個Mobile SDK實作[!DNL Adobe Target]。

## 電子郵件實作

[**電子郵件概述**](implement/email/overview.md)：如何在電子郵件中實作[!DNL Adobe Target]的概述。

## API指南

[**簡介**](before-administer/target-api-overview.md)： [!DNL Adobe Target] API概觀。

[**[!DNL Target Delivery API]**](/help/dev/implement/delivery-api/overview.md)：使用[!DNL Adobe Target]傳遞API在網頁和行動頻道以及非瀏覽器型IoT裝置（例如連線電視、資訊站或店內數位熒幕）之間傳遞體驗。

[**[!DNL Target Admin API]**](administer/admin-api/admin-api-overview-new.md)：使用[!DNL Adobe Target] Admin API來管理屬性、活動、對象、選件、屬性、報告、mboxes、主機、環境等。

[**[!DNL Target Profile API]**](/help/dev/administer/profile-api/profiles-api.md)：擷取[!DNL Adobe Target]使用者設定檔資訊。

[**[!DNL Target Reporting API]**](https://developer.adobe.com/target/administer/admin-api/#tag/Reports)：擷取[!UICONTROL A/B Test]和[!UICONTROL Automated Personalization]活動報告資料。

[**[!DNL Target Recommendations API]**](https://developer.adobe.com/target/administer/recommendations-api/)：使用[!DNL Target Recommendations] API。

[**[!DNL Target Models API]**](administer/models-api/models-api-overview.md)：管理封鎖清單以定義[!DNL Target]機器學習模型中使用的功能。

[**Admin ConsoleAPI**](https://developer.adobe.com/umapi/)：透過Adobe「使用者管理」和「使用者同步API」來管理使用者和產品權益。

[**[!DNL Adobe Experience Platform Edge Network Server API]**](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/overview.html)：將[!DNL Adobe Experience Platform Edge Network Server] API用於各種資料收集、個人化、廣告和行銷使用案例。

## 資源

* [Adobe開放原始碼存放庫](https://github.com/adobe)
* [目標節點JS SDK Source](https://github.com/adobe/target-nodejs-sdk)
* [目標節點JS SDK範例Repo](https://github.com/adobe/target-nodejs-sdk-samples)
* [Target Java SDK Source](https://github.com/adobe/target-java-sdk)
* [目標Java SDK範例存放庫](https://github.com/adobe/target-java-sdk-samples)
* [Target實作](./before-implement/prepare-to-implement-target.md)
* [Target管理](./before-administer/target-api-overview.md)
* [Adobe Target開發檔案GitHub存放庫](https://github.com/AdobeDocs/target-developers)
* [Adobe Target發行說明](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html)
* [Adobe Target商業使用手冊](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=zh-Hant)

