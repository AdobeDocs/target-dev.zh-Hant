---
keywords: 行動應用程式，常見問題集，常見問題集， target行動應用程式
description: 檢視行動應用程式的常見問題及其關於 [!DNL Adobe Target] 的答案。
title: 什麼是行動應用程式的常見問題 [!DNL About Target] ？
feature: Implement Mobile
exl-id: 06cae3de-83a4-4018-a832-66fb292a1d0f
TQID: https://experienceleague.adobe.com/DvyEuK-o-3qAJDKVI3tSe9ZirSQa0my6vbIhJQP9EU4
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: d051910f-2bda-47ea-a969-6ade9fcd71f1
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 316
ht-degree: 3%

---

# 適用於行動應用程式的[!DNL Target]常見問題集

關於行動應用程式[!DNL Target]常見問題的清單。

## 我應該使用[!DNL Adobe Experience Platform]中的標籤來部署SDK，還是可以不使用Launch部署SDK？

[Adobe Marketing Cloud Git](https://github.com/Adobe-Marketing-Cloud/acp-sdks/){target=_blank}提供SDK。 如果您未使用Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html){target=_blank}中的[標籤，則必須在您的應用程式中管理您自己的設定檔案並管理它。

## 目前提供哪些SDK？

Adobe Experience Platform Mobile SDK目前支援iOS、Android和React。 如需詳細資訊，請參閱[Adobe Experience Cloud Platform Mobile SDK指南](https://experienceleague.adobe.com/docs/mobile.html){target=_blank}。

## 根據經緯度的驗證，以位置為基礎的功能頻率為何？

如需詳細資訊，請參閱[Adobe Places檔案](https://experienceleague.adobe.com/docs/places/using/home.html){target=_blank}。

## Adobe Experience Platform Mobile SDK是否需要at.js才能運作？

不需要，您不需要at.js才能使用行動SDK。 at.js是網站的[!DNL Target] JavaScript資料庫。 Adobe Experience Platform Mobile SDK適用於行動應用程式。

## [!DNL Target] Mobile是否只是[!DNL Adobe Target] Premium Product SKU的功能？

無. 對於[!DNL Adobe Target Standard]客戶，您只能將Mobile SDK用於[!UICONTROL A/B測試]和[!UICONTROL 體驗鎖定目標] (XT)活動，與[!DNL Target Standard]行動應用程式附加元件搭配使用。 如果您想要在行動應用程式中使用[!UICONTROL Recommendations]或AI支援的功能，您需要[Adobe Target Premium](https://experienceleague.adobe.com/docs/target/using/introduction/intro.html#premium)授權。

## [!DNL Adobe Experience Manager] (AEM)與[!DNL Target]個行動裝置活動之間是否有行動應用程式整合？

目前，您可以從AEM將JSON [體驗片段](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html){target=_blank}共用給[!DNL Target]，然後可能將其用於行動應用程式活動。
