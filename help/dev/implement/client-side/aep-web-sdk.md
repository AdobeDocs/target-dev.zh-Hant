---
keywords: Adobe Experience Platform Web SDK， aep web sdk， web sdk， sdk， adobe experience cloud， platform edge network， adobe experience platform edge network， edge network， aep edge network， Adobe Experience Platform Web SDK0
description: 瞭解如何使用 [!UICONTROL Adobe Experience Platform Web SDK] 與 [!UICONTROL Adobe Experience Cloud] 透過 [!UICONTROL AEP Edge Network].
title: 如何使用實作 [!UICONTROL Experience PlatformWeb SDK]？
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 13%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] (AEP Web SDK)是使用者端的JavaScript程式庫，可讓客戶 [!UICONTROL Adobe Experience Cloud] 與 [!DNL Adobe Experience Cloud] (包括 [!DNL Target])，透過 [!UICONTROL Adobe Experience Platform Edge Network]. 除了JavaScript程式庫，還有 [!UICONTROL Adobe Experience Platform] 擴充功能可協助處理您的Web SDK設定。

如需詳細資訊，請參閱 *[!UICONTROL Adobe Experience Platform Web SDK]* 說明：

* 如需完整資訊： [什麼是 [!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)
* 有關的特定資訊 [!DNL Target]： [[!DNL Target] 概觀](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html)

## 教學課程

下列教學課程可協助您實作：

### 實作 [!DNL Adobe Experience Cloud] 替換為 [!DNL Platform Web SDK]

瞭解如何實作 [!DNL Experience Cloud] 應用程式使用 [!DNL Adobe Experience Platform Web SDK] 替換為 [本教學課程](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=zh-Hant). 有關的特定資訊 [!DNL Target]，請參閱教學課程中標題為 [設定 [!DNL Target] 使用Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html).

### 移轉 [!DNL Target] 從at.js 2.*x* 至 [!DNL Platform Web SDK]

瞭解如何移轉您的 [!DNL Target] 從at.js 2.*x* 至 [!DNL Adobe Experience Platform Web SDK] 替換為 [本教學課程](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html).

## 建議文件

除了 [!UICONTROL Platform Web SDK] 上述檔案，本指南中的主題也包含特定於 [!UICONTROL Platform Web SDK] 因為它與 [!DNL Target] 特色與功能。

| 功能 | 說明/連結 |
| --- | --- |
| [活動 QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) | 在中使用品質保證URL [!DNL Target] 透過永不變更的預覽連結、可選對象鎖定目標以及與即時活動資料保持分段的QA報表，輕鬆執行端對端活動QA。 活動QA可讓您完全測試 [!DNL Target] 活動之前，請將其啟動為上線。<p>另請參閱 [Target JavaScript程式庫QA模式相容性](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#compatibility) 和 [預覽URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#preview). |
| [[!UICONTROL Analytics for Target] (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) | [!UICONTROL 目標的Adobe Analytics] (A4T)是一種跨解決方案的整合，可讓您根據以下專案建立活動： [!DNL Analytics] 轉換量度和受眾區段。 A4T 整合可讓您使用 Analytics 報表來檢查成果。<p>另請參閱 [支援的活動型別](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html#section_F487896214BF4803AF78C552EF1669AA) 和 [Adobe Experience Platform Web SDK實作的實作步驟](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html#platform). |
| [對象](https://experienceleague.adobe.com/docs/target/using/audiences/target.html) | 中的對象 [!DNL Target] 決定哪些人可以看到鎖定目標活動中的內容與體驗。<p>另請參閱 [使用對象清單](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#use-list) 和 [合併多個對象](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html). |
| [建立對象](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html??lang=zh-Hant) | 使用在 [!DNL Adobe Experience Platform] 中建立的對象可提供更豐富的客戶資料，進而帶來更具影響力的個人化。 <p>另請參閱 [使用來自Adobe Experience Platform的受眾](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#aep). |
| [優惠決定](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html) | 新增在中建立的優惠決定 [!DNL Adobe Journey Optimizer] 至 [!DNL Target] 活動（手動A/B測試或體驗鎖定目標）來決定並為您的網頁和行動裝置上的訪客提供下一個最佳選件。 |
| [重新導向優惠方案 - A4T 常見問題](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html) | 重新導向選件會使訪客的瀏覽器重新導向至新頁面。<p>另請參閱 [此 [!UICONTROL Adobe Experience Platform Web SDK] 支援A4T的重新導向選件嗎？](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html#platform) |
| [回應權杖](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) | 回應Token允許您傳送 [!DNL Target] 資料與Google Analytics和其他協力廠商整合。<p>另請參閱 [透過Platform Web SDK傳送資料給Google Analytics](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html#sending-data-to-google-analytics-via-platform-web-sdk) 以檢視如何完成此工作的範常式式碼。 |
| [實作單頁應用程式](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html) 在 *[!UICONTROL Platform Web SDK] 概述* 指南。 | [!UICONTROL Adobe Experience Platform Web SDK] 提供豐富的功能，讓貴公司能以新世代使用者端技術(例如單頁應用程式(SPA))為基礎進行個人化。 |
| [TLS (傳輸層安全性) 加密變更](../../before-implement/tls-transport-layer-security-encryption.md) | TLS （傳輸層安全性）可協助您維持最高安全性標準，提升客戶資料的安全性。 |
