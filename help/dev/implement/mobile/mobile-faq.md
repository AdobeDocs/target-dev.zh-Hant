---
keywords: 行動應用程式，常見問題集，常見問題集， target行動應用程式
description: 檢視常見問題集及其答案 [!DNL Adobe Target] 適用於行動應用程式。
title: 常見問題集 [!DNL About Target] 適用於行動應用程式嗎？
feature: Implement Mobile
exl-id: 06cae3de-83a4-4018-a832-66fb292a1d0f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 5%

---

# [!DNL Target](適用於行動應用程式) 常見問答

常見問題集清單 [!DNL Target] 適用於行動應用程式。

## 我是否應在以下位置使用標籤： [!DNL Adobe Experience Platform] 以部署SDK，或是在不使用Launch的情況下部署SDK嗎？

SDK位於 [Adobe Marketing Cloud git](https://github.com/Adobe-Marketing-Cloud/acp-sdks/){target=_blank}. If you don't use [tags in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html){target=_blank}，您必須自行管理設定檔案，並在應用程式中加以管理。

## 目前提供哪些SDK？

Adobe Experience Platform Mobile SDK目前支援iOS、Android和React。 如需詳細資訊，請參閱 [Adobe Experience Cloud Platform Mobile SDK指南](https://experienceleague.adobe.com/docs/mobile.html){target=_blank}.

## 根據經緯度的驗證，以位置為基礎的功能頻率為何？

請參閱 [Adobe Places檔案](https://experienceleague.adobe.com/docs/places/using/home.html){target=_blank} 以取得詳細資訊。

## Adobe Experience Platform Mobile SDK是否需要at.js才能運作？

不需要，您不需要at.js才能使用行動SDK。 at.js是 [!DNL Target] 適用於網站的JavaScript資料庫。 Adobe Experience Platform Mobile SDK適用於行動應用程式。

## 是 [!DNL Target] 行動功能 [!DNL Adobe Target] 僅限進階產品SKU？

無.的 [!DNL Adobe Target Standard] 客戶，您可以將我們的行動SDK用於 [!UICONTROL A/B測試] 和 [!UICONTROL 體驗鎖定] (XT)活動僅使用 [!DNL Target Standard] 行動應用程式附加元件。 如果您想使用 [!UICONTROL Recommendations] 或行動應用程式中的AI支援功能，您需要 [Adobe Target Premium](https://experienceleague.adobe.com/docs/target/using/introduction/intro.html#premium) 授權。

## 以下兩者之間是否有行動應用程式整合 [!DNL Adobe Experience Manager] (AEM)和 [!DNL Target] 行動裝置活動？

目前，您可以共用JSON [體驗片段](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html){target=_blank} 從AEM到 [!DNL Target] 而且可能將其用於行動應用程式活動。
