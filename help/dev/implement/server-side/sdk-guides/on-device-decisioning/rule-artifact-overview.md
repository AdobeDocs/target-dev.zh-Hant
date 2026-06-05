---
title: 瞭解裝置上決策規則成品
description: 瞭解如何使用規則成品，這是 [!DNL Adobe Target] [!UICONTROL 裝置上決策]活動的JSON表示法。
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
TQID: https://experienceleague.adobe.com/mPzCK-vBYFAQnslX-8FPsBaeSiYtyxjZv76anbpHWuE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 86209eb483ca69d40615c632ba435d27fec78f36
workflow-type: tm+mt
source-wordcount: 277
ht-degree: 0%

---

# 規則成品概觀

規則成品是[!DNL Adobe Target] [!UICONTROL 裝置上決策]活動的JSON表示法。 它是由[!DNL Adobe Target]產生並傳播至Akamai CDN，以確保有儘可能接近使用者的規則成品。 其中包含中繼資料，可確保準確執行和傳送您的活動，同時允許透過事件追蹤進行即時分析。 [!DNL Adobe Target] SDK的設定方式可允許自動管理規則成品，並可根據使用者指定的時間間隔下載或更新成品。 此外，您也可以使用分散式記憶體快取系統（例如[Memcached](https://memcached.org/)）來維護您自己的規則成品本機復本，以初始化[!DNL Adobe Target] SDK，讓您的無狀態伺服器可以立即處理要求。 若要深入瞭解這些選項，請參閱下列指南：

* [透過 [!DNL Adobe Target] SDK自動下載、儲存及更新規則成品](rule-artifact-sdk.md)
* [透過JSON裝載下載、儲存和更新規則成品](rule-artifact-json.md)

## 規則成品範例

按一下這裡以取得[規則成品](rule-artifact-example.md)的範例。

## 如何檢視使用者端的規則成品

啟用追蹤將會從[!DNL Adobe Target]輸出有關規則成品（特別是URL）的其他資訊。

1. 導覽至Target UI。

   <!-- Insert image-target-ui-1.png -->
   ![替代影像](assets/asset-rule-artifact-1.png)

1. 瀏覽至&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]**，然後按一下&#x200B;**[!UICONTROL 產生新的授權權杖]**。

   <!-- Insert image-target-ui-2.png -->
   ![替代影像](assets/asset-rule-artifact-2.png)

1. 將新產生的授權權杖複製到剪貼簿，並將其新增到您的Target請求。

   ```javascript {line-numbers="true"}
   const request = {
     trace: {
       authorizationToken: '88f1a924-6bc5-4836-8560-2f9c86aeb36b'
     },
     execute: {
       mboxes: [{
         address: getAddress(req),
         name: "node-sdk-mbox"
       }]
   }};
   ```

1. 透過終端機輸出Target追蹤，以檢視成品的相關詳細資訊。 可透過`artifactLocation`變數存取URL。

   ```
   "trace": {
     "clientCode": "your-client-code",
     "artifact": {
       "artifactLocation": "https://assets.adobetarget.com/your-client-code/production/v1/rules.bin",
       "pollingInterval": 300000,
       "pollingHalted": false,
       "artifactVersion": "1.0.0",
       "artifactRetrievalCount": 10,
       "artifactLastRetrieved": "2020-09-20T00:09:42.707Z",
        "clientCode": "your-client-code",
      "environment": "production",
       "generatedAt": "2020-09-22T17:17:59.783Z"
     },
   ```
