---
keywords: 伺服器端，伺服器端， sdk， sdk，裝置上，決策，裝置上， ondevice，零延遲，延遲，近零， node.js，伺服器端3
description: 瞭解如何使用 [!UICONTROL [!UICONTROL on-device decisioning]]快取您的 [!DNL Target] 在伺服器上執行A/B和MVT活動，以便在幾乎零延遲的情況下執行記憶體內決策。
title: 什麼是裝置上決策？
feature: Implement Server-side
exl-id: 22ed3072-56f0-4075-9d1a-d642afe3b649
source-git-commit: ff0becf3fe3a6fd6694e13243b6a93b910316434
workflow-type: tm+mt
source-wordcount: '1022'
ht-degree: 9%

---

# 裝置上決策概覽

新一代 [!DNL Adobe Target] SDK現在提供 [!UICONTROL on-device decisioning]，可在伺服器上快取您的A/B和Experience Targeting (XT)行銷活動，並以接近零延遲的速度執行記憶體內決策，而不會封鎖對的網路請求 [!DNL Adobe Target]的Edge Network。

[!DNL Adobe Target] 也提供彈性，可透過即時伺服器呼叫，從您的實驗和ML驅動的個人化行銷活動中，提供最相關和最新的體驗。 換言之，當效能最重要時，您可以選擇利用 [!UICONTROL on-device decisioning]，但若需要最相關且最新的體驗，則可改為進行伺服器呼叫。 另請參閱 [何時使用裝置上決策與邊緣決策](../../sdk-guides/on-device-decisioning/supported-features.md) 以瞭解應優先使用其中一個專案的使用案例。

>[!NOTE]
>
>裝置上決策可用於使用者端及伺服器端實施。 本文說明 [!UICONTROL on-device decisioning] 用於伺服器端。 有關以下專案的資訊： [!UICONTROL on-device decisioning] 若是使用者端，請參閱使用者端實作檔案 [此處](../../../client-side/atjs/on-device-decisioning/on-device-decisioning.md).

## 如何運作？

當您安裝和初始化 [!DNL Adobe Target] SDK搭配 [!UICONTROL on-device decisioning] 已啟用， a *規則成品* 會從最靠近伺服器的Akamai CDN在本機下載並快取。 當要求擷取 [!DNL Adobe Target] 體驗是在伺服器端應用程式中進行，而系統會根據快取規則成品中編碼的中繼資料(定義您的所有 [!UICONTROL on-device decisioning] A/B和XT活動。

下圖顯示 [!UICONTROL on-device decisioning] 架構。 按一下以展開影像。

（按一下影像可展開至完整寬度。）

![裝置上決策架構圖](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/assets/asset-sdk-local-decisioning-architecture-diagram.png "裝置上決策架構圖"){zoomable=&quot;yes&quot;}

## 有哪些優點？

* **提供幾乎零延遲的決策。** 在記憶體和裝置上執行分組和決策，以避免封鎖網路請求。
* **增強應用程式效能。** 執行實驗並為您的客戶和使用者提供個人化，而不會損害一般使用者體驗。
* **改善Google網站品質分數。** 透過在記憶體和伺服器端進行決策，可改善您線上業務的Google網站品質分數，讓消費者更容易發現它。
* **向即時分析學習。** 透過即時瞭解您的活動績效 [!DNL Adobe Target] 或A4T報表，讓您在關鍵時刻樞紐分析策略。

## 支援的功能

### 活動

裝置上決策支援以下建立的活動型別 [表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)：

* [!UICONTROL A/B Test]
* [!UICONTROL Experience Targeting] (XT)

### 配置方法

裝置上決策支援下列配置方法：

* 手動

### 對象目標定位

裝置上決策支援下列對象規則：

| 對象規則 | 裝置上決策 |
| --- | --- |
| [地理](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | 是<P>使用裝置上決策時，支援下列地理屬性：<ul><li>國家/地區</li><li>城市</li><li>緯度</li><li>經度</li></ul> |
| [網路](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | 否 |
| [行動](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | 否 |
| [自訂引數](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | 是 |
| [作業系統 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | 是 |
| [網頁](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | 是 |
| [瀏覽器](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | 是 |
| [訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | 否 |
| [流量來源](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | 否 |
| [時間範圍](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | 是 |
| [Experience Cloud對象](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Adobe Audience Manager、Adobe Analytics和Adobe Experience Manager的受眾 | 否 |

## 我要如何布建使用者端以使用 [!UICONTROL on-device decisioning]？

裝置上決策功能適用於所有使用者 [!DNL Adobe Target] 使用的客戶 [!DNL Adobe Target] 伺服器端SDK。 若要啟用此功能，請瀏覽至 **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** 在 [!DNL Adobe Target] UI，並啟用 **[!UICONTROL On-Device Decisioning]** 切換。

>[!NOTE]
>
>您必須擁有管理員或核准者 *使用者角色* 啟用或停用 [!UICONTROL On-Device Decisioning] 切換。

![替代影像](assets/asset-odd-toggle.png)

啟用「裝置上決策」切換後， [!DNL Adobe Target] 將開始產生和傳播 *規則人工因素* 適用於您的使用者端。

>[!NOTE]
>
>在初始化 [!DNL Adobe Target] 要使用的SDK [!UICONTROL on-device decisioning]. 規則成品必須先產生並傳播至Akamai CDN，才能用於 [!UICONTROL on-device decisioning] 才能運作。

### 包含所有現有 [!UICONTROL on-device decisioning] 成品切換中的合格活動

切換此專案 **於** 您想要即時使用的時間 [!DNL Target] 符合資格的活動 [!UICONTROL on-device decisioning] 自動包含在成品中。

離開此切換 **關閉** 表示您需要重新建立並啟用任何 [!UICONTROL on-device decisioning] 活動，以便將其納入產生的規則成品中。

## 我如何知道活動是 [!UICONTROL on-device decisioning] 功能強大？

建立活動後，標籤為 **[!UICONTROL Decisioning Method]**，會顯示在活動詳細資訊頁面中，指出活動是否為 [!UICONTROL on-device decisioning] 功能強大。

![替代影像](assets/asset-odd9.png)

您也可以檢視所有符合以下條件的活動： [!UICONTROL on-device decisioning] 功能於 **[!UICONTROL Activities]** 新增欄以建立頁面 **[!UICONTROL Decisioning Method]** 至活動清單。

![替代影像](assets/asset-odd7.png)

>[!NOTE]
>
>建立和啟動符合以下條件的活動後： [!UICONTROL on-device decisioning] 支援，可能需要20分鐘，才會納入產生並傳播至Akamai CDN PoP的規則成品中。

## 需要遵循的步驟摘要是什麼，才能確保我的 [!UICONTROL on-device decisioning] 活動透過以下方式成功傳遞： [!DNL Adobe Target]的伺服器端SDK？

1. 存取 [!DNL Adobe Target] UI並導覽至 **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** 以啟用 **[!UICONTROL On-Device Decisioning]** 切換。
1. 啟用 **[!UICONTROL Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact]** 切換。
1. 建立及啟用所支援的活動型別 [!UICONTROL on-device decisioning]，並確認 **[!UICONTROL Decisioning Method]** 是 **[!UICONTROL On-Device Decisioning]** 用於該活動。
1. 安裝並初始化 [Node.js](../../node-js/overview.md) 或 [Java](../../java/overview.md) SDK搭配 `decisioningMethod = on-device`.
1. 實作 `getOffers()` 或 `getAttributes()` 在程式碼中擷取裝置上體驗。
1. 部署您的程式碼。

如需示範如何開始使用上述步驟1至3的範例，請參閱 [快速入門](../getting-started/getting-started.md) 區段。


## 其他資源

### 網路研討會影片：利用 [!DNL Adobe Target] 的裝置上決策在零延遲的情況下進行個人化和測試

市場人員、產品擁有者和開發人員被要求在網站、應用程式，以及與客戶連結的任何地方最佳化整體客戶體驗，此情況更勝以往。具有資料獨立單位和複雜實作的多種工具是不夠的。

在這場錄製的網路研討會中， [!DNL Adobe Target] 產品專家會討論將關鍵的體驗最佳化決策移到裝置上，以便在幾乎無延遲的情況下於本機執行，可以促成令人興奮的新使用案例，同時為客戶改善網站效能。

>[!VIDEO](https://video.tv.adobe.com/v/328148/?quality=12)


### 教學課程：裝置上決策

[!DNL Adobe Target] [!UICONTROL on-device decisioning] 可幾乎零延遲的內容傳送。

這段7分鐘的影片：

* 說明 [!UICONTROL on-device decisioning]，包括與其他方法的比較 [!DNL Target] 實施
* 示範如何啟用 [!UICONTROL on-device decisioning] 在Target中
* 檢查已設定JSON內容的表單式撰寫器活動範例
* 顯示Node.JS SDK程式碼範例，其中包含下列專案所需的機碼組態： [!UICONTROL on-device decisioning]
* 在瀏覽器中示範結果

>[!VIDEO](https://video.tv.adobe.com/v/329032/?quality=12)

如需更多影片和教學課程，請參閱 [[!DNL Adobe Target] Tutorials](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html).

### Adobe技術部落格 — 第1部分：執行 [!DNL Adobe Target] 適用於Edge平台實驗與個人化的NodeJS SDK （Akamai Edge工作者）

[按一下這裡以存取部落格](https://medium.com/adobetech/part-1-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-4d8660964ed9).

### Adobe 技術部落格 - 第 2 部分：在 Edge 平台上執行 [!DNL Adobe Target] NodeJS SDK 以進行測試和個人化 (AWS Lambda@Edge)

[按一下這裡以存取部落格](https://medium.com/adobetech/part-2-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-aws-4d6bdac24563).
