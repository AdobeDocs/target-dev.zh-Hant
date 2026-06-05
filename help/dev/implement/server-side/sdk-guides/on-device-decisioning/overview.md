---
keywords: 伺服器端，伺服器端， sdk， sdk，裝置上，決策，裝置上， ondevice，零延遲，延遲，近零， node.js，伺服器端3
description: 瞭解如何使用[!UICONTROL [！UICONTROL裝置上決策]]在伺服器上快取您的 [!DNL Target] A/B和MVT活動，以便在幾乎零延遲的情況下執行記憶體內決策。
title: 什麼是裝置上決策？
feature: Implement Server-side
exl-id: 22ed3072-56f0-4075-9d1a-d642afe3b649
TQID: https://experienceleague.adobe.com/-HHGn3lG5fOh2GLXQ6jOLRQmX7H24lN-2fseOg4y5H4
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: adee20bd-51f4-461d-b9db-d215f8756eebid: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bcc5edb5-84c3-4940-9f84-ed88b6c16274id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e0eb8757-182f-49f3-94a4-1587d16f5094id: e1e0219c-f879-479f-8427-888ed2a6e9c2id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1349
ht-degree: 8%

---

# 裝置上決策概觀

新一代[!DNL Adobe Target] SDK現在提供[!UICONTROL 裝置上決策]，可在伺服器上快取您的A/B和體驗鎖定目標(XT)行銷活動，並在幾乎零延遲的情況下執行記憶體內決策，而不會封鎖對[!DNL Adobe Target]的Edge Network的網路要求。

[!DNL Adobe Target]也能透過即時伺服器呼叫，靈活地從您的實驗和ML驅動的個人化行銷活動中，提供最相關和最新的體驗。 換言之，當效能最重要的時候，您可以選擇使用[!UICONTROL 裝置上決策]，但是當需要最相關且最新的體驗時，可以改用伺服器呼叫。 檢視[何時使用裝置上決策與邊緣決策](../../sdk-guides/on-device-decisioning/supported-features.md)，以瞭解有關使用其中一個優先於另一個的使用案例。

>[!NOTE]
>
>裝置上決策可用於使用者端及伺服器端實施。 本文說明伺服器端的[!UICONTROL 裝置上決策]。 如需有關使用者端[!UICONTROL 裝置上決策]的資訊，請參閱使用者端實作檔案[這裡](../../../client-side/atjs/on-device-decisioning/on-device-decisioning.md)。

## 如何運作？

當您安裝並初始化已啟用[!UICONTROL 裝置上決策]的[!DNL Adobe Target] SDK時，會從最靠近伺服器的Akamai CDN下載&#x200B;*規則成品*&#x200B;並在伺服器上本機快取。 在您的伺服器端應用程式中提出擷取[!DNL Adobe Target]體驗的請求時，系統會根據快取規則成品中編碼的中繼資料，在記憶體中決定要傳回哪些內容，該決定會定義您的所有[!UICONTROL 裝置上決策] A/B和XT活動。

下圖顯示[!UICONTROL 裝置上決策]架構。 按一下以展開影像。

（按一下影像可展開至完整寬度。）

![裝置上決策架構圖](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/assets/asset-sdk-local-decisioning-architecture-diagram.png "裝置上決策架構圖"){zoomable="yes"}

## 有哪些優點？

* **提供幾乎零延遲的決策。** 在記憶體和裝置上執行分組和決策，以避免封鎖網路請求。
* **增強應用程式效能。** 執行實驗並為您的客戶和使用者提供個人化，而不會損害一般使用者體驗。
* **改善Google網站品質分數。** 透過在記憶體和伺服器端進行決策，可改善您線上業務的Google網站品質分數，讓消費者更容易發現它。
* **向即時分析學習。** 透過[!DNL Adobe Target]或A4T報表即時取得活動績效的深入分析，讓您在關鍵時刻樞紐分析策略。

## 支援的功能

### 活動

裝置上決策支援[表單式體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)建立的下列活動型別：

* [!UICONTROL A/B 測試]
* [!UICONTROL 體驗鎖定] (XT)

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
| [Experience Cloud Audiences](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) （來自Adobe Audience Manager、Adobe Analytics和Adobe Experience Manager的受眾） | 否 |

## 我要如何布建使用者端以使用[!UICONTROL 裝置上決策]？

使用[!DNL Adobe Target]伺服器端SDK的所有[!DNL Adobe Target]客戶都可使用裝置上決策。 若要啟用此功能，請在[!DNL Adobe Target] UI中導覽至&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]** > **[!UICONTROL 帳戶詳細資料]**，然後啟用&#x200B;**[!UICONTROL 裝置上決策]**&#x200B;切換功能。

>[!NOTE]
>
>您必須擁有管理員或核准者&#x200B;*使用者角色*，才能啟用或停用[!UICONTROL 裝置上決策]切換功能。

![替代影像](assets/asset-odd-toggle.png)

啟用「裝置上決策」切換後，[!DNL Adobe Target]將開始為您的使用者端產生和傳播&#x200B;*規則成品*。

>[!NOTE]
>
>在初始化[!DNL Adobe Target] SDK以使用[!UICONTROL 裝置上決策]之前，請確定您已啟用切換功能。 規則成品必須先產生並傳播至Akamai CDN，才能進行[!UICONTROL 裝置上決策]。

### 在成品切換中包含所有現有的[!UICONTROL 裝置上決策]合格活動

當您想要所有符合[!UICONTROL 裝置上決策]的即時[!DNL Target]活動自動包含在成品中時，請在&#x200B;**切換此**。

將此切換&#x200B;**保持關閉**&#x200B;表示您需要重新建立並啟用任何[!UICONTROL 裝置上決策]活動，才能將其包含在產生的規則成品中。

## 我如何知道活動具有[!UICONTROL 裝置上決策]功能？

建立活動後，活動詳細資訊頁面中顯示的名為&#x200B;**[!UICONTROL 決策方法]**&#x200B;的標籤會指出活動是否具有[!UICONTROL 裝置上決策]功能。

![替代影像](assets/asset-odd9.png)

您也可以在&#x200B;**[!UICONTROL 活動]**&#x200B;頁面上看到[!UICONTROL 裝置上決策]功能的所有活動，只要將欄&#x200B;**[!UICONTROL 決策方法]**&#x200B;新增到活動清單中即可。

![替代影像](assets/asset-odd7.png)

>[!NOTE]
>
>建立並啟動具有[!UICONTROL 裝置上決策]功能的活動後，可能需要20分鐘才能納入產生並傳播至Akamai CDN PoP的規則成品中。

## 為確保我的[!UICONTROL 裝置上決策]活動透過[!DNL Adobe Target]的伺服器端SDK成功傳送，所需遵循的步驟摘要為何？

1. 存取[!DNL Adobe Target] UI並導覽至&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]** > **[!UICONTROL 帳戶詳細資料]**&#x200B;以啟用&#x200B;**[!UICONTROL 裝置上決策]**&#x200B;切換功能。
1. 啟用成品&#x200B;]**切換中的**[!UICONTROL &#x200B;包含所有現有的[!UICONTROL 裝置上決策]合格活動。
1. 建立並啟用[!UICONTROL 裝置上決策]支援的活動型別，並驗證該活動的&#x200B;**[!UICONTROL 決策方法]**&#x200B;是&#x200B;**[!UICONTROL 裝置上決策]**。
1. 使用`decisioningMethod = on-device`安裝並初始化[Node.js](../../node-js/overview.md)或[Java](../../java/overview.md) SDK。
1. 在您的程式碼中實作`getOffers()`或`getAttributes()`以擷取裝置上的體驗。
1. 部署您的程式碼。

如需示範如何開始使用上述步驟1-3的範例，請參閱[快速入門](../getting-started/getting-started.md)區段。


## 其他資源

### 網路研討會影片：利用 [!DNL Adobe Target] 的裝置上決策在零延遲的情況下進行個人化和測試

市場人員、產品擁有者和開發人員被要求在網站、應用程式，以及與客戶連結的任何地方最佳化整體客戶體驗，此情況更勝以往。 具有資料獨立單位和複雜實作的多種工具是不夠的。

在這個錄製的網路研討會中，[!DNL Adobe Target]產品專家會討論將關鍵體驗最佳化決策移到裝置上，以便在幾乎無延遲的情況下於本機執行，可以促成令人興奮的新使用案例，同時為客戶改善網站效能。

>[!VIDEO](https://video.tv.adobe.com/v/328148/?quality=12)


### 教學課程：裝置上決策

[!DNL Adobe Target] [!UICONTROL 裝置上決策]啟用幾乎零延遲的內容傳遞。

這段7分鐘的影片：

* 說明[!UICONTROL 裝置上決策]，包括它與[!DNL Target]實作的其他方法的比較
* 示範如何在Target中啟用[!UICONTROL 裝置上決策]
* 檢查已設定JSON內容的表單式撰寫器活動範例
* 顯示Node.JS SDK程式碼範例，其中包含[!UICONTROL 裝置上決策]所需的金鑰組態
* 在瀏覽器中示範結果

>[!VIDEO](https://video.tv.adobe.com/v/329032/?quality=12)

如需更多影片和教學課程，請參閱[[!DNL Adobe Target] 教學課程](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html)。

### Adobe技術部落格 — 第1部分：在Edge平台上執行[!DNL Adobe Target] NodeJS SDK以進行測試和個人化（Akamai Edge工作者）

[按一下這裡以存取部落格](https://medium.com/adobetech/part-1-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-4d8660964ed9)。

### Adobe 技術部落格 - 第 2 部分：在 Edge 平台上執行 [!DNL Adobe Target] NodeJS SDK 以進行測試和個人化 (AWS Lambda@Edge)

[按一下這裡以存取部落格](https://medium.com/adobetech/part-2-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-aws-4d6bdac24563)。
