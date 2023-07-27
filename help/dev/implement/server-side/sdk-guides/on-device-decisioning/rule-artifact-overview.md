---
title: 瞭解裝置上決策規則成品
description: 瞭解如何使用規則成品，它是您的 [!DNL Adobe Target] [!UICONTROL 裝置上決策] 活動。
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# 規則成品概觀

規則成品是您的JSON表示法 [!DNL Adobe Target] [!UICONTROL 裝置上決策] 活動。 產生者 [!DNL Adobe Target] 並傳播至Akamai CDN，以確保有儘可能接近使用者的規則成品。 其中包含中繼資料，可確保準確執行和傳送您的活動，同時允許透過事件追蹤進行即時分析。 此 [!DNL Adobe Target] SDK的設定方式可允許自動管理規則成品，而成品可依使用者指定的時間間隔下載或更新。 此外，您也可以使用分散式記憶體快取系統（例如），維護您自己的規則成品本機副本 [Memcached](https://memcached.org/) 初始化 [!DNL Adobe Target] SDK，讓您的無狀態伺服器能立即處理要求。 若要深入瞭解這些選項，請參閱下列指南：

* [透過自動下載、儲存和更新規則成品 [!DNL Adobe Target] SDK](rule-artifact-sdk.md)
* [透過JSON裝載下載、儲存和更新規則成品](rule-artifact-json.md)

## 規則成品範例

按一下這裡以取得 [規則成品](rule-artifact-example.md).

## 如何檢視使用者端的規則成品

啟用追蹤將會輸出其他資訊，來自 [!DNL Adobe Target] 與規則成品（尤其是URL）相關。

1. 導覽至Target UI。

   &lt;! — 插入image-target-ui-1.png —>
   ![替代影像](assets/asset-rule-artifact-1.png)

1. 瀏覽至 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** 並按一下 **[!UICONTROL 產生新的授權權杖]**.

   &lt;! — 插入image-target-ui-2.png —>
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

1. 透過終端機輸出Target追蹤，以檢視成品的相關詳細資訊。 可透過以下方式存取URL： `artifactLocation` 變數中。

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
