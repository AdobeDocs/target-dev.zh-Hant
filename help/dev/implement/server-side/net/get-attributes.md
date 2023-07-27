---
title: 使用getAttributes於 [!DNL Adobe Target] 搭配.NET SDK
description: 瞭解如何使用getAttributes()來擷取實驗和個人化體驗，從 [!DNL Target] 和擷取屬性值。
feature: APIs/SDKs
exl-id: 808da83d-3077-468b-a2ad-e35c25905f7d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 10%

---

# 取得屬性(.NET)

## 說明

`GetAttributes()` 用於擷取實驗和個人化體驗 [!DNL Target] 和擷取屬性值。

## 方法

### getAttributes

```dotnet {line-numbers="true"}
TargetAttributes TargetClient.GetAttributes(TargetDeliveryRequest targetRequest, params string[] mboxes)
```

## 參數

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | 否 | null | 相同 [!DNL Target] 請求已用於 [取得優&#x200B;惠](get-offers.md) |
| mboxNames | 引數字串[] | 否 | null | mbox名稱的引數陣列 |

## 結果

A `TargetAttributes` 物件傳回自 `TargetClient.GetAttributes()` 具有以下屬性和方法：

| 屬性/方法 | 傳回類型 | 說明 |
| --- | --- | --- |
| 回應 | TargetDeliveryResponse | 傳回通常由傳回的回應物件 [取得優惠方案](get-offers.md) |
| ToDictionary | IReadOnlyDictionary | 傳回字典的字典，其中索引鍵值配對按mbox名稱分組 |
| ToMboxDictionary(mboxName) | IReadOnlyDictionary | 傳回所提供mbox之索引鍵值配對的字典 |
| GetBoolean(mboxName， key， defaultValue) | bool | 傳回指定mbox名稱和屬性索引鍵的值 |
| GetString(mboxName， key， defaultValue) | string | 傳回指定mbox名稱和屬性索引鍵的值 |
| GetInteger(mboxName， key， defaultValue) | int | 傳回指定mbox名稱和屬性索引鍵的值 |
| GetDouble(mboxName， key， defaultValue) | 兩次 | 傳回指定mbox名稱和屬性索引鍵的值 |
| GetValue(mboxName， key， defaultValue) | T | 傳回指定mbox名稱和屬性索引鍵的值 |

## 範例

### \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .Build();

var offerAttributes = targetClient.GetAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
var searchProviderId = offerAttributes.GetString("demo-engineering-flags", "searchProviderId");

//returns a simple Dictionary representing the mbox offer
var engineeringFlags = offerAttributes.ToMboxDictionary("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

var assetUrl = $"http://{engineeringFlags["cdnHostname"]}/path/to/asset";
```
