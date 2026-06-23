---
keywords: 實作， javascript程式庫， js， atjs，裝置上決策，裝置上決策， at.js，裝置上，裝置上，疑難排解，疑難排解，實作2
description: 瞭解如何使用at.js資料庫疑難排解[!UICONTROL 裝置上決策]。
title: 如何透過at.js JavaScript程式庫疑難排解裝置上決策？
feature: at.js
exl-id: b9530cc7-5e83-4fdf-bde9-b2492e0861ff
TQID: https://experienceleague.adobe.com/Ji3jAHC0Ek7FrVnabEEMm-KCtxJLJ5rSz4uyi6sWpiE
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 281
ht-degree: 0%

---

# 疑難排解at.js的[!UICONTROL 裝置上決策]

完成下列步驟，以使用at.js JavaScript程式庫疑難排解[!UICONTROL Adobe Target]中的[!UICONTROL 裝置上決策]：

## 步驟1：啟用at.js的主控台記錄檔

附加URL引數`mboxDebug=1`可讓at.js在瀏覽器的主控台中列印訊息。

所有訊息都包含「AT：」前置詞，以便於概觀。 為確保成功載入成品，您的主控台記錄應包含類似下列的訊息：

```
AT: LD.ArtifactProvider fetching artifact - https://assets.adobetarget.com/your-client-cide/production/v1/rules.json
AT: LD.ArtifactProvider artifact received - status=200
```

下圖顯示主控台記錄檔中的這些訊息：

（按一下影像可展開至完整寬度。）

![含有成品訊息的主控台記錄檔](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/browser-console.png "含有成品訊息的主控台記錄檔"){zoomable="yes"}

## 步驟2：驗證瀏覽器網路標籤中的規則成品下載

開啟瀏覽器的「網路」標籤。

例如，若要在Google Chrome中開啟DevTools：

1. 按Ctrl+Shift+J (Windows)或Command+Option+J (Mac)。
1. 瀏覽至「網路」標籤。
1. 依關鍵字「rules.json」篩選您的呼叫，以確保只顯示成品rules檔案。

   此外，您可以依「/delivery|rules.json/」篩選，以顯示所有Target呼叫和成品rules.json。

   Google Chrome中的![網路索引標籤](assets/rule-json.png)

## 步驟3：使用at.js自訂事件驗證規則成品下載

at.js程式庫會傳送兩個新的自訂事件，以支援[!UICONTROL 裝置上決策]。

* `adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`
* `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`

您可以訂閱以在應用程式中監聽這些自訂事件，以在成品rules檔案下載成功或失敗時採取行動。

下列範例顯示聆聽成品下載成功和失敗事件的程式碼範例：

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED, function(e) { 
  console.log("Artifact successfully downloaded", e.detail);
}, false);

document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_FAILED, function(e) { 
  console.log("Artifact failed to download", e.detail);
}, false);
```

