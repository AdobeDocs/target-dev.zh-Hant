---
keywords: at.js 整合, 支援的整合, 不支援的整合, 第三方整合
description: 請參閱所支援（和不支援）的整合 [!DNL Adobe Target] at.js，包括 [!UICONTROL 目標分析] (A4T)、 [!UICONTROL Experience CloudID服務]、等等。
title: at.js支援哪些整合？
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 60%

---

# at.js 整合

關於與 [!DNL Adobe Target] 的常見整合及其對 at.js 支援狀態的資訊。

若您迫切需要此處不支援或未提到的整合，請聯絡您的客戶代表或顧問。

## 支援的整合

| 整合 | 詳細資料 |
|--- |--- |
| [!UICONTROL Analytics for Target] (A4T) | 請參閱 [Adobe Analytics 做為 Adobe Target (A4T) 的報表來源](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)。 |
| [!UICONTROL 設定檔和對象] (P&amp;A) | 另請參閱 [受眾](https://experienceleague.adobe.com/docs/core-services/interface/audiences/audience-library.html??lang=zh-Hant) 在 *核心服務使用手冊*. |
| [!UICONTROL Experience Cloud ID 服務] | 請參閱 [Adobe Experience Cloud ID 服務文件](https://experienceleague.adobe.com/docs/id-service/using/home.html)。 |
| [!UICONTROL Adobe Experience Platform中的標籤] | [!UICONTROL Adobe Experience Platform中的標籤] 是推出的新一代標籤管理功能 [!DNL Adobe]. [!UICONTROL 標籤] 可讓客戶透過簡單的方式部署及管理必要的分析、行銷及廣告整合功能，以便支援相關客戶體驗。 另請參閱 [實作 [!DNL Target] 使用Adobe Experience Platform](../how-to-deployatjs/implement-target-using-adobe-launch.md). |
| [!UICONTROL Adobe Experience Manager] (AEM)Cloud Service | 此 [!UICONTROL AEM Cloud Service] 可建立 [!UICONTROL A/B測試] 和 [!UICONTROL 體驗鎖定] 活動在AEM工作流程中。 透過支援at.js [!UICONTROL Adobe Experience Manager] 6.2含FP-11577 （或更新版本）。 如需詳細資訊，請參閱[與 整合 [!DNL Adobe Target]，並選取您的 AEM 版本。](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html) |
| [!UICONTROL AEM 體驗片段] | 在中於AEM建立的體驗片段 [!DNL Target] 活動可讓您將AEM的易用性和威力，結合中強大的自動化智慧(AI)和機器學習(ML)功能 [!DNL Target] 大規模測試並個人化體驗。  AEM 將您的所有內容和資產集中在一個中央位置，以支援您的個人化策略。AEM 可讓您在一個位置中輕鬆地為桌上型電腦、平板電腦和行動裝置建立內容，不必撰寫程式碼。不需要為每個裝置建立頁面，AEM會自動根據您的內容調整每個體驗。  請參閱 [AEM 體驗片段](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html)。 |

## 不支援的整合

| 整合 | 詳細資料 |
|--- |--- |
| 舊版 [!DNL Target] 至 [!DNL SiteCatalyst] 整合 | 這是透過頁面呼叫將行銷活動和配方 id 傳送至 [!DNL SiteCatalyst] 的整合，讓您能夠在 [!DNL SiteCatalyst] UI 中執行報表。A4T 已取代此功能。 |
| 舊版 [!DNL Target] 至 [!DNL SiteCatalyst] 整合 | 這是提出 mobx 呼叫 `"SiteCatalyst: Event"` 和 `"SiteCatalyst: Purchase"` 的整合，讓您能夠根據 evars 和 props 來建立成功量度和使用者設定檔。A4T 和 P&amp;A 已取代此功能。 |
| 舊版 [!DNL Audience Manager] (AAM)至 [!DNL Target] 整合 | 此整合可提出前端 API 呼叫來擷取 AAM 區段，然後在頁面上的每一個 mbox 呼叫上當作 mbox 參數傳送。 |

## 第三方整合

| 整合 | 詳細資料 |
|--- |--- |
| 其他標記管理員 | at.js 應該可以與非 Adobe 標記管理平台一起使用，但在使用其他廠商所開發的自訂整合功能時請小心。他們的整合可能依賴 at.js 中已不存在的內部 mbox.js 函式。 |
| 第三方資料提供者 (例如，Demandbase、Bluekai、天氣 API) | 可以使用 at.js [資料提供者](../atjs-functions/targetglobalsettings.md#data-providers)功能，與許多用於補充 Target 使用者設定檔的第三方資料提供者整合。。 |
