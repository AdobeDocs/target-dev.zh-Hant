---
title: 使用getAttributes於 [!DNL Adobe Target] 搭配Java SDK
description: 瞭解如何使用getAttributes()來擷取實驗和個人化體驗，從 [!DNL Target] 和擷取屬性值。
feature: APIs/SDKs
exl-id: e493e1b9-7180-4a7c-b98d-be84cc3a57c3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 13%

---

# 取得屬性(Java)

## 說明

`getAttributes()` 用於擷取實驗和個人化體驗 [!DNL Target] 和擷取屬性值。

## 方法

### getAttributes

```javascript {line-numbers="true"}
Attributes TargetClient.getAttributes(TargetDeliveryRequest targetRequest, String ...mboxes)
```

## 參數

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | 是 | 無 | 使用的相同Target請求 [取得優&#x200B;惠](get-offers.md) |
| mboxNames | var-args陣列 | 否 | 無 | mbox名稱的變數陣列 |


## 結果

一個 `Attributes` 物件傳回自 `TargetClient.getAttributes()` 方法如下：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| getBoolean(mboxName， key) | 布林值 | 傳回指定mbox名稱和屬性索引鍵的值 |
| getString(mboxName， key) | 字串 | 傳回指定mbox名稱和屬性索引鍵的值 |
| getInteger(mboxName， key) | 整數 | 傳回指定mbox名稱和屬性索引鍵的值 |
| getDouble(mboxName， key) | 雙倍 | 傳回指定mbox名稱和屬性索引鍵的值 |
| toMboxMap(mboxName) | 地圖 | 傳回包含索引鍵值組的簡單對應 |
| getResponse() | TargetDeliveryResponse | 傳回getOffers通常會傳回的回應物件 |

## 範例

### Java

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .build();

Attributes offerAttributes = targetJavaClient.getAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
String searchProviderId = offerAttributes.getString("demo-engineering-flags", "searchProviderId");

//returns a simple Map representing the mbox offer
Map<String, Object> engineeringFlags = offerAttributes.toMboxMap("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

String assetUrl = "http://" + engineeringFlags.cdnHostname + "/path/to/asset";
```
