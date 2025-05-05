---
keywords: Adobe Experience Platform Web SDK， aep web sdk， web sdk， sdk， adobe experience cloud， platform edge network， adobe experience platform edge network， edge network， aep edge network， Adobe Experience Platform Web SDK0
description: 瞭解如何使用[!UICONTROL Adobe Experience Platform Web SDK]透過[!UICONTROL AEP Edge Network]與[!UICONTROL Adobe Experience Cloud]中的各種服務互動。
title: 如何使用[!UICONTROL Experience Platform Web SDK]實作？
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 8%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] (AEP Web SDK)是使用者端的JavaScript資料庫，可讓[!UICONTROL Adobe Experience Cloud]的客戶透過[!UICONTROL Adobe Experience Platform Edge Network]與[!DNL Adobe Experience Cloud] （包括[!DNL Target]）中的各種服務互動。 除了JavaScript程式庫，還有[!UICONTROL Adobe Experience Platform]擴充功能可協助您進行Web SDK設定。

如需詳細資訊，請參閱&#x200B;*[!UICONTROL Adobe Experience Platform Web SDK]*&#x200B;說明中的下列連結：

* 如需完整資訊： [什麼是[!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)
* 有關[!DNL Target]的特定資訊： [[!DNL Target] 概觀](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html)

## 教學課程

下列教學課程可協助您實作：

### 使用[!DNL Platform Web SDK]實作[!DNL Adobe Experience Cloud]

瞭解如何使用[!DNL Adobe Experience Platform Web SDK]搭配[本教學課程](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=zh-Hant)實作[!DNL Experience Cloud]應用程式。 如需[!DNL Target]的特定資訊，請參閱教學課程中標題為[使用Platform Web SDK設定 [!DNL Target] ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html)的部分。

### 從at.js 2.[!DNL Target]*x*&#x200B;至[!DNL Platform Web SDK]

瞭解如何從at.js 2.[!DNL Target]*x*&#x200B;與[!DNL Adobe Experience Platform Web SDK]搭配此教學課程[&#128279;](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html)。

## 建議文件

除了上述[!UICONTROL Platform Web SDK]檔案之外，本指南中的主題也包含特定於[!UICONTROL Platform Web SDK]的資訊，因為它與[!DNL Target]特色與功能有關。

| 功能 | 說明/連結 |
| --- | --- |
| [活動 QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) | 在[!DNL Target]中使用QA URL來執行簡易的端對端活動QA，同時具有永不變更的預覽連結、可選對象鎖定目標以及與即時活動資料保持分段的QA報表。 活動QA可讓您在啟動[!DNL Target]活動之前，完整測試這些活動。<p>請參閱[Target JavaScript資料庫QA模式相容性](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#compatibility)和[預覽URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#preview)。 |
| [[!UICONTROL Analytics for Target] (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) | [!UICONTROL Adobe Analytics for Target] (A4T) 是一種跨解決方案的整合，可讓您根據 [!DNL Analytics] 轉換量度和客群區段來建立活動。A4T整合可讓您使用Analytics報表來檢查結果。<p>請參閱[支援的活動型別](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html#section_F487896214BF4803AF78C552EF1669AA)和[Adobe Experience Platform Web SDK實作的實作步驟](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html#platform)。 |
| [客群](https://experienceleague.adobe.com/docs/target/using/audiences/target.html) | [!DNL Target]中的對象會決定哪些人可以看到鎖定目標活動中的內容與體驗。<p>請參閱[使用對象清單](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#use-list)和[結合多個對象](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html)。 |
| [建立客群](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html??lang=zh-Hant) | 使用在[!DNL Adobe Experience Platform]中建立的對象可提供更豐富的客戶資料，從而帶來更具影響力的個人化。<p>請參閱[使用來自Adobe Experience Platform](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#aep)的對象。 |
| [優惠決定](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html) | 將在[!DNL Adobe Journey Optimizer]中建立的報價決定新增至[!DNL Target]個活動（手動A/B測試或體驗鎖定目標），以決定並為您的網路和行動裝置上的訪客提供下一個最佳報價。 |
| [重新導向優惠方案 - A4T 常見問題](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html) | 重新導向選件會使訪客的瀏覽器重新導向至新頁面。<p>請參閱[ [!UICONTROL Adobe Experience Platform Web SDK]是否支援A4T的重新導向選件？](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html#platform) |
| [回應權杖](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) | 回應Token可讓您將[!DNL Target]資料傳送至Google Analytics和其他協力廠商整合。<p>請參閱[透過Platform Web SDK傳送資料給Google Analytics](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html#sending-data-to-google-analytics-via-platform-web-sdk)，檢視如何完成此工作的程式碼範例。 |
| 在&#x200B;*[!UICONTROL Platform Web SDK]總覽*&#x200B;指南中的[單頁應用程式實作](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html)。 | [!UICONTROL Adobe Experience Platform Web SDK]提供豐富的功能，讓貴公司能以新世代使用者端技術(例如單頁應用程式(SPA))為基礎進行個人化。 |
| [TLS (傳輸層安全性) 加密變更](../../before-implement/tls-transport-layer-security-encryption.md) | TLS （傳輸層安全性）可協助您維持最高安全性標準，提升客戶資料的安全性。 |
