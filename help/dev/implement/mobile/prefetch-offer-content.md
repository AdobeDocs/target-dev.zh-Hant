---
keywords: 選件，預先擷取， iOS， android， sdk，行動，行動sdk， 8美元
description: 使用iOS和Android Mobile SDK中的 [!DNL Adobe Target] 預先擷取功能，透過快取伺服器回應，儘量以最少次數擷取選件。
title: 我可以預先擷取行動應用程式的選件內容嗎？
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 43%

---

# 預先擷取選件內容

[!DNL Target] 預先擷取功能會使用 iOS 和 Android Mobile SDK，透過快取伺服器回應盡量以最少次數擷取選件。

>[!IMPORTANT]
>
>支援[!DNL Adobe Mobile]版本4。*x* SDK已於2021年8月31日結束，不建議再供[!DNL Adobe Target]個行動使用者使用。
>
>[適用於行動應用程式的Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank}是支援行動應用程式中[!DNL Adobe Experience Cloud]解決方案和服務的建議解決方案。

此程序會減少載入時間，避免多個網路呼叫，並允許通知 [!DNL Target] 行動應用程式使用者造訪哪一個 mbox。預先擷取呼叫期間，會擷取並快取所有內容，而且會從快取中擷取此內容，以供包含指定 mbox 名稱之快取內容的所有未來呼叫使用。

搭配iOS和Android Mobile SDK使用預先擷取方法時，請考量下列限制：

* 預先擷取內容不會在跨啟動之間持續有效。只要應用程式仍然存在，或直到呼叫 `clearPrefetchCache()` 方法為止，則會快取預先擷取內容。
* [!UICONTROL Auto-Allocate]和[!UICONTROL Auto-Target]流量配置方法、[!UICONTROL Automated Personalization]或[!UICONTROL Recommendations]活動型別，或A/B或XT活動](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html)中的[建議選件不支援預先擷取功能。

如需詳細資訊，包括預先提取方法、公用類別和程式碼範例，請參閱:

* **iOS：** [在&#x200B;*Mobile Services iOS SDK說明*&#x200B;中，預先擷取iOS中的選件內容](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html)。
* **Android：** [在&#x200B;*Mobile Services Android SDK說明*&#x200B;中，預先擷取Android中的選件內容](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html)。
