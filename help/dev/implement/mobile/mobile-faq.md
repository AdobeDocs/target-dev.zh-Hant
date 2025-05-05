---
keywords: 行動應用程式，常見問題集，常見問題集， target行動應用程式
description: 檢視行動應用程式的常見問題及其關於 [!DNL Adobe Target] 的答案。
title: 什麼是行動應用程式的常見問題 [!DNL About Target] ？
feature: Implement Mobile
exl-id: 06cae3de-83a4-4018-a832-66fb292a1d0f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 1%

---

# 適用於行動應用程式的[!DNL Target]常見問題集

關於行動應用程式[!DNL Target]常見問題的清單。

## 我應該使用[!DNL Adobe Experience Platform]中的標籤來部署SDK，還是可以不使用Launch來部署SDK？

SDK可在[Adobe Marketing Cloud Git](https://github.com/Adobe-Marketing-Cloud/acp-sdks/){target=_blank}上取得。 如果您未使用Adobe Experience Platform[&#128279;](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=zh-Hant){target=_blank}中的標籤，則必須在您的應用程式中管理您自己的設定檔案並管理它。

## 目前提供哪些SDK？

Adobe Experience Platform Mobile SDK目前支援iOS、Android和React。 如需詳細資訊，請參閱[Adobe Experience Cloud平台Mobile SDK指南](https://experienceleague.adobe.com/docs/mobile.html?lang=zh-Hant){target=_blank}。

## 根據經緯度的驗證，以位置為基礎的功能頻率為何？

如需詳細資訊，請參閱[Adobe地標檔案](https://experienceleague.adobe.com/docs/places/using/home.html?lang=zh-Hant){target=_blank}。

## Adobe Experience Platform Mobile SDK是否需要at.js才能運作？

不需要，您不需要at.js才能使用行動SDK。 at.js是網站的[!DNL Target] JavaScript資料庫。 Adobe Experience Platform Mobile SDK適用於行動應用程式。

## [!DNL Target] Mobile是否只是[!DNL Adobe Target] Premium Product SKU的功能？

無.對於[!DNL Adobe Target Standard]客戶，您只能將[!UICONTROL A/B Test]和[!UICONTROL Experience Targeting] (XT)活動的行動SDK與[!DNL Target Standard]行動應用程式附加元件搭配使用。 如果您想要在行動應用程式中使用[!UICONTROL Recommendations]或AI支援的功能，您需要[Adobe Target Premium](https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=zh-Hant#premium)授權。

## [!DNL Adobe Experience Manager] (AEM)與[!DNL Target]個行動裝置活動之間是否有行動應用程式整合？

目前，您可以從AEM將JSON [體驗片段](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html?lang=zh-Hant){target=_blank}共用至[!DNL Target]，然後可能會將其用於行動應用程式活動。
