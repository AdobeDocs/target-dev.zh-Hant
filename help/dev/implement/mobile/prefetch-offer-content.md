---
keywords: 選件，預先擷取， iOS， android， sdk，行動，行動sdk， 8美元
description: 使用 [!DNL Adobe Target] iOS和Android Mobile SDK中的預先擷取功能可透過快取伺服器回應，儘量以最少次數擷取選件。
title: 我可以預先擷取行動應用程式的選件內容嗎？
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 39%

---

# 預先擷取選件內容

[!DNL Target] 預先擷取功能會使用 iOS 和 Android Mobile SDK，透過快取伺服器回應盡量以最少次數擷取選件。

>[!IMPORTANT]
>
>支援 [!DNL Adobe Mobile] 版本4。*x* SDK已於2021年8月31日結束，不建議再用於 [!DNL Adobe Target] 行動使用者。
>
>此 [適用於行動應用程式的Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} 是推薦的供電解決方案 [!DNL Adobe Experience Cloud] 行動應用程式中的解決方案和服務。

此程序會減少載入時間，避免多個網路呼叫，並允許通知 [!DNL Target] 行動應用程式使用者造訪哪一個 mbox。預先擷取呼叫期間，會擷取並快取所有內容，而且會從快取中擷取此內容，以供包含指定 mbox 名稱之快取內容的所有未來呼叫使用。

搭配iOS和Android Mobile SDK使用預先擷取方法時，請考量下列限制：

* 預先擷取內容不會在跨啟動之間持續有效。只要應用程式仍然存在，或直到呼叫 `clearPrefetchCache()` 方法為止，則會快取預先擷取內容。
* 不支援預先擷取功能 [!UICONTROL 自動分配] 和 [!UICONTROL 自動鎖定目標] 流量分配方法，用於 [!UICONTROL Automated Personalization] 或 [!UICONTROL Recommendations] 活動型別，或 [A/B或XT活動中的Recommendations選件](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html).

如需詳細資訊，包括預先提取方法、公用類別和程式碼範例，請參閱:

* **iOS：**  [iOS中的預先擷取選件內容](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html) 在 *Mobile Services iOS SDK說明*.
* **Android：**  [預先擷取Android中的選件內容](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html) 在 *Mobile Services Android SDK說明*.
