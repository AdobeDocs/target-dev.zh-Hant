---
title: 透過Java SDK在 [!DNL Adobe Target] 中使用getAttributes
description: 瞭解如何使用getAttributes()從 [!DNL Target] 擷取實驗性和個人化體驗，並擷取屬性值。
feature: APIs/SDKs
exl-id: e493e1b9-7180-4a7c-b98d-be84cc3a57c3
TQID: https://experienceleague.adobe.com/ZZy9nUXiyR-qwBmOgv-TPS6ZuilvAuW850gH1Doqquo
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 169
ht-degree: 13%

---

# 取得屬性(Java)

## 說明

`getAttributes()`是用來從[!DNL Target]擷取實驗與個人化體驗，以及擷取屬性值。

## 方法

### getAttributes

```javascript {line-numbers="true"}
Attributes TargetClient.getAttributes(TargetDeliveryRequest targetRequest, String ...mboxes)
```

## 參數

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | 是 | 無 | 與[取得選件{&#x200B;1}使用的目標要求相同](get-offers.md) |
| mboxNames | var-args陣列 | 無 | 無 | mbox名稱的變數陣列 |


## 結果

從`TargetClient.getAttributes()`傳回的`Attributes`物件具有以下方法：

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
