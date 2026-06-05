---
keywords: at.js 整合, 支援的整合, 不支援的整合, 第三方整合
description: 檢視 [!DNL Adobe Target] at.js支援（且不支援）的整合，包括[!UICONTROL Analytics for Target] (A4T)、[!UICONTROL Experience Cloud ID服務]等等。
title: at.js支援哪些整合？
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
TQID: https://experienceleague.adobe.com/RdcxcIGufo2O5aKPqIAJVINkCzZ1Brcv8EXiX1n4buc
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: adee20bd-51f4-461d-b9db-d215f8756eebid: c93393a4-e558-47e1-992e-c91ed4d480ceid: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2: id: df62f171-ac37-440f-8f0f-f41a72ebdd34id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: e0eb8757-182f-49f3-94a4-1587d16f5094id: e6ff21d3-dec6-4298-8590-7c749fffaf78id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 530
ht-degree: 50%

---

# at.js 整合

關於與 [!DNL Adobe Target] 的常見整合及其對 at.js 支援狀態的資訊。

若您迫切需要此處不支援或未提到的整合，請聯絡您的客戶代表或顧問。

## 支援的整合

| 整合 | 詳細資料 |
|--- |--- |
| [!UICONTROL Analytics for Target] (A4T) | 請參閱 [Adobe Analytics 做為 Adobe Target (A4T) 的報表來源](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)。 |
| [!UICONTROL 設定檔和對象] (P&amp;A) | 請參閱&#x200B;*核心服務使用手冊*&#x200B;中的[對象](https://experienceleague.adobe.com/docs/core-services/interface/audiences/audience-library.html??lang=zh-Hant)。 |
| [!UICONTROL Experience Cloud ID 服務] | 請參閱 [Adobe Experience Cloud ID 服務文件](https://experienceleague.adobe.com/docs/id-service/using/home.html)。 |
| Adobe Experience Platform中的[!UICONTROL 標籤] | Adobe Experience Platform]中的[!UICONTROL 標籤是新一代[!DNL Adobe]標籤管理功能。 [!UICONTROL 標籤]可讓客戶透過簡單的方式部署及管理必要的分析、行銷及廣告標籤功能，以便支援相關客戶體驗。 請參閱[使用Adobe Experience Platform](../how-to-deployatjs/implement-target-using-adobe-launch.md)實作 [!DNL Target] 。 |
| [!UICONTROL Adobe Experience Manager] (AEM) Cloud Service | [!UICONTROL AEM Cloud Service]可以在AEM工作流程中建立[!UICONTROL A/B測試]和[!UICONTROL 體驗鎖定目標]活動。 支援at.js搭配[!UICONTROL Adobe Experience Manager] 6.2搭配FP-11577 （或更新版本）。 如需詳細資訊，請參閱[與 [!DNL Adobe Target]整合](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)，並選取您的AEM版本。 |
| [!UICONTROL AEM 體驗片段] | 在[!DNL Target]活動的AEM中建立的體驗片段，可讓您將AEM的易用性和威力，結合[!DNL Target]中強大的自動化智慧(AI)和機器學習(ML)功能，以大規模測試並個人化體驗。  AEM 將您的所有內容和資產集中在一個中央位置，以支援您的個人化策略。 AEM 可讓您在一個位置中輕鬆地為桌上型電腦、平板電腦和行動裝置建立內容，不必撰寫程式碼。 不需要為每個裝置建立頁面，AEM會自動根據您的內容調整每個體驗。  請參閱 [AEM 體驗片段](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html)。 |

## 不支援的整合

| 整合 | 詳細資料 |
|--- |--- |
| 舊版[!DNL Target]與[!DNL SiteCatalyst]整合 | 這是透過頁面呼叫將行銷活動和配方ID傳送至[!DNL SiteCatalyst]的整合，讓您能夠在[!DNL SiteCatalyst] UI中執行報表。 A4T 已取代此功能。 |
| 舊版[!DNL Target]與[!DNL SiteCatalyst]整合 | 這是提出 mobx 呼叫 `"SiteCatalyst: Event"` 和 `"SiteCatalyst: Purchase"` 的整合，讓您能夠根據 evars 和 props 來建立成功量度和使用者輪廓。 A4T 和 P&amp;A 已取代此功能。 |
| 舊版[!DNL Audience Manager] (AAM)與[!DNL Target]整合 | 此整合可提出前端 API 呼叫來擷取 AAM 區段，然後在頁面上的每一個 mbox 呼叫上當作 mbox 參數傳送。 |

## 第三方整合

| 整合 | 詳細資料 |
|--- |--- |
| 其他標記管理員 | at.js 應該可以與非 Adobe 標記管理平台一起使用，但在使用其他廠商所開發的自訂整合功能時請小心。 他們的整合可能依賴 at.js 中已不存在的內部 mbox.js 函式。 |
| 第三方資料提供者 (例如，Demandbase、Bluekai、天氣 API) | 使用at.js [資料提供者](../atjs-functions/targetglobalsettings.md#data-providers)功能，可以整合許多用於補充Target使用者設定檔的第三方資料提供者。 |
