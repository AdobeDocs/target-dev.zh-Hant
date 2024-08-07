---
title: 如何在 [!DNL Adobe Target] Node.js SDK中使用非同步要求
description: 瞭解 [!DNL Target] Node.js SDK如何支援非同步要求，將有效目標時間減少為零。
feature: APIs/SDKs
exl-id: aa06f3ca-7d2a-4334-8092-730a8705dfb0
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 18%

---

# 取得屬性(Node.js)

## 說明

`[!UICONTROL getAttributes()]`是用來從[!DNL Target]擷取實驗與個人化體驗，以及擷取屬性值。

## 方法

### getAttributes

```js {line-numbers="true"}
TargetClient.getAttributes(mboxNames: Array, options: Object): Promise
```

## 參數

| 名稱 | 類型 | 必要 | 預設值 |
| --- | --- | --- |--- |
| mboxNames | 陣列 | 是 | 無 |
| options | 物件 | 無 | 無 |

## Promise

`TargetClient.getAttributes()`傳回的`Promise`會使用下列方法解析物件：

| 方法 | 傳回類型 | 說明 |
| --- | --- | --- |
| getValue(mboxName， key) | 任何 | 傳回指定mbox名稱和屬性索引鍵的值 |
| asObject(mboxName) | 物件 | 傳回具有索引鍵值配對的簡單json物件 |
| getResponse() | [getOffers回應](https://github.com/jasonwaters/target-nodejs-sdk#targetclientgetoffers) | 傳回`getOffers`通常會傳回的回應物件 |

## 範例

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);
const offerAttributes = await targetClient.getAttributes(["demo-engineering-flags"]);


//returns just the value of searchProviderId from the mbox offer
const searchProviderId = offerAttributes.getValue("demo-engineering-flags", "searchProviderId");

//returns a simple JSON object representing the mbox offer
const engineeringFlags = offerAttributes.asObject("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

const assetUrl = `http://${engineeringFlags.cdnHostname}/path/to/asset`;
```
