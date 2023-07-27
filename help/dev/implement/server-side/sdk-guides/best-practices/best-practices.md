---
title: 使用裝置上決策時的最佳實務
description: 瞭解使用的最佳實務 [!UICONTROL 裝置上決策] 在 [!DNL Adobe Target]
feature: Implement Server-side
exl-id: a0ca014d-ad9f-4ecc-961d-cb7ba236507f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 2%

---

# 最佳做法

[!DNL Adobe] 建議使用時遵循下列最佳實務 [!UICONTROL 裝置上決策]：

## 決策方法為「裝置上」時的最佳實務

當使用「裝置上」作為決策方法時，會在訪客首次載入網頁時下載成品。 任何需要在第一個頁面載入時發生的活動資格（無快取），都只有在成品完全下載後才會發生。 您可以遵循某些最佳實務，以確保新的匿名訪客可快速取得活動資格。

* 停用不在成品中的具有「裝置上」功能的活動。
* 如果您有Target Premium，則可使用 [屬性/工作區](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html??lang=zh-Hant) 為不同的工作區建立不同的成品檔案。
* 如果您的成品檔案由於合法原因變得非常大，您可以使用「混合」決策方法。 此方法可讓您平行下載成品，所有Target API呼叫都會線上上進行，直到下載成品為止。 請閱讀以下「混合」決策模式的最佳實務區段，以進一步瞭解此方法。
* 如果您有單頁應用程式(SPA)， [!DNL Adobe] 建議您在首次載入頁面期間，先載入並初始化at.js，然後再載入應用程式的主JavaScript檔案。 此方法可更早啟動成品下載，提供更快速的體驗呈現。

## 決策方法為「混合」時的最佳實務

使用「混合」作為決策方法時，成品會同時下載。 下載成品之前，任何 [!DNL Target] 即使「位置」支援在裝置上，API呼叫仍會透過線路進行。 此行為是所有使用者的預設行為 `getOffers()` 會在大多數情況下呼叫並提供最佳效能。 如果您變更的預設行為 `getOffers()` 藉由設定 `decisioningMethod` 至 `on-device`，請遵循這些最佳實務來避免錯誤並確保最佳效能。

* 如果您決定呼叫 `getOffers()` 替換為 `decisioningMethod` 作為 `on-device` 第一次載入頁面時，您必須在「ARTIFACT_DOWNLOAD_SUCCEEDED」at.js事件處理常式中執行此動作以避免錯誤。 如果您的成品非常大，則任何使用此方法的「位置」都必須在成品完全下載後才會呈現，這可能會延遲體驗呈現。 [!DNL Adobe] 建議您儘量不要使用此方法。 使用此方法時，請遵循上文「裝置上」最佳實務區段底下的最佳實務，以縮小成品大小。
