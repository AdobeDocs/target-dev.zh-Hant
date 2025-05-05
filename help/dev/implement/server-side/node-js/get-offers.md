---
title: 使用Node.js SDK時，在 [!DNL Adobe Target] 中使用[!UICONTROL getOffers()]
description: 瞭解如何使用[!UICONTROL getOffers()]執行決定並從 [!DNL Adobe Target]擷取體驗。
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 21%

---

# [!UICONTROL Get Offers] (Node.js)

## 說明

`[!UICONTROL getOffers()]`用於執行決定並從[!DNL Adobe Target]擷取體驗。


## 方法

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## 參數

`options`物件具有下列結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- |--- | --- | --- | --- |
| 請求 | 物件 | 是 | 無 | 符合[[!DNL Target] 傳送API](/help/dev/implement/delivery-api/overview.md)要求 |
| visitorCookie | 字串 | 無 | 無 | ECID (VisitorId) Cookie |
| targetCookie | 字串 | 無 | 無 | [!DNL Target] Cookie |
| targetLocationHint | 字串 | 無 | 無 | [!DNL Target]位置提示 |
| consumerId | 字串 | 否 | 無 | [!UICONTROL Analytics for Target] (A4T)拼接的consumerIds |
| CustomerIds | 陣列 | 否 | 無 | VisitorId相容格式的客戶ID |
| sessionId | 字串 | 無 | 無 | 用於連結多個[!DNL Target]請求 |
| 訪客 | 物件 | 無 | 新VisitorId | 提供外部VisitorId例項 |

## Promise

`Promise`傳回的結構如下：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 請求 | 物件 | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)個要求 |
| 回應 | 物件 | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)個回應 |
| visitorState | 物件 | 應傳遞給訪客API `getInstance()`的物件 |
| targetCookie | 物件 | [!DNL Target] Cookie |
| targetLocationHintCookie | 物件 | [!DNL Target]位置提示Cookie |
| analyticsDetails | 陣列 | 使用使用者端Analytics時的Analytics裝載 |
| responseTokens | 陣列 | [回應Token](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=zh-Hant&)的清單。 |
| trace | 陣列 | 所有請求mbox/檢視的彙總追蹤資料 |
| 狀態 | 物件 | 包含回應狀態的物件。 |
| 決策方法 | 字串 | 決定要使用的決策方法（[裝置上](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、伺服器端、混合式） |

用來將資料傳回瀏覽器的`targetCookie`和`targetLocationHintCookie`物件具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 名稱 | 字串 | Cookie 名稱 |
| value | 任何 | Cookie值，則會轉換為字串 |
| maxAge | 數字 | `maxAge`選項可方便您設定相對於目前時間（以秒為單位）的過期時間 |

用於表示目標回應狀態的`status`物件具有下列結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 狀態 | 數字 | HTTP狀態代碼 |
| 訊息 | 字串 | 有關回應的訊息。 例如，它可以指出回應是決定[裝置上](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)還是伺服器端 |
| remoteMboxes | 陣列 | 當決定方法為`on-device`時，會提供無法完全決定裝置上的mbox名稱陣列。 換句話說，需要[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)要求。 |

## 範例

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    context: {channel: "web"},
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
}};

const response = await targetClient.getOffers({ request });
```
