---
title: Adobe Target傳送API通知
description: 如何使用引發通知 [!UICONTROL Adobe Target傳送API]？
keywords: 傳送api
exl-id: 711388fd-2c1f-4ca4-939f-c56dc4bdc04a
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# 通知

當使用者造訪或呈現預先擷取的mbox或檢視時，應觸發通知。

若要針對正確的mbox或檢視觸發通知，請務必追蹤對應的 `eventToken` 用於每個mbox或檢視。 具有正確內容的通知 `eventToken` 必須引發對應的mbox或檢視，才能正確反映報表。

## 預先擷取Mbox的通知

可透過單一傳遞呼叫傳送一或多個通知。 判斷需要追蹤的量度是否為 `click` 或 `display` ，以便每個mbox `type` 可以正確反映通知。 此外，傳入 `id` 每個通知，以判斷是否透過正確傳送通知[!UICONTROL  Adobe Target傳送API]. 此 `timestamp` 轉送至也很重要 [!DNL Target] 以指示何時 `click` 或 `display` 針對指定mbox發生，以用於報表用途。

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '{
    "id": {
      "tntId": "abcdefghijkl00023.1_1"
    },
    "context": {
      "channel": "web",
      "browser" : {
        "host" : "demo"
      },
      "address" : {
        "url" : "http://demo.dev.tt-demo.com/demo/store/index.html"
      },
      "screen" : {
        "width" : 1200,
        "height": 1400
      }
    },
      "notifications": [
      {
      "id" : "SummerOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerShoesOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerShoesOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerDressOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerDressOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
    } 
    ]
  }'
```

上述呼叫範例會產生一個回應，指出 `notifications` 已成功處理要求。

```
{
  "status": 200,
  "requestId": "36014eed-4772-4c48-a9e2-e532762b6a85",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "notifications": [
      {
          "id": "SummerOfferNotification"
      },
      {
          "id": "SummerDressOfferNotification"
      },
      {
          "id": "SummerShoesOfferNotification"
      }
  ]
}
```

如果所有 `notifications` 傳送至 [!DNL Target] 正確處理之後，它們會顯示在 `notifications` 陣列做為回應。 但是，如果 `notifications` `id` 遺失，該特定 `notification` 未通過。 在此案例中，重試邏輯可實施，直到成功為止 `notification` 已擷取回應。 請確定重試邏輯已指定逾時，以便API呼叫不會封鎖並導致效能延遲。

## 預先擷取檢視的通知

可透過單一傳遞呼叫傳送一或多個通知。 判斷需要追蹤的量度是否為 `click` 或 `display` ，以便能正確反映通知的型別。 此外，傳入 `id` 每個通知，以判斷是否透過正確傳送通知 [!UICONTROL Adobe Target傳送API]. 時間戳記也很重要才能轉送到 [!DNL Target] 以指示何時 `click` 或 `display` 發生於指定檢視中，以用於報表用途。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
  },
  "context": {
    "channel": "web",
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "notifications": [{
      "id": "228",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "checkout-express",
      }
    },
    {
      "id": "5",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "home",
      }
    },
    {
      "id": "6",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "products",
      }
    }
  ]
}'
```

上述呼叫範例會產生一個回應，指出 `notifications` 已成功處理要求。

```
{
    "status": 200,
    "requestId": "85cc7394-c19a-4398-9b8b-bbee1e4c4579",
    "client": "demo",
    "id": {
        "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "notifications": [
        {
            "id": "5"
        },
        {
            "id": "6"
        },
        {
            "id": "228"
        }
    ]
}
```

如果所有 `notifications` 傳送至  [!DNL Target] 正確處理之後，它們會顯示在 `notifications` 陣列做為回應。 但是，如果 `notifications` `id` 遺失，該特定通知未通過。 在此案例中，重試邏輯可實施，直到擷取成功的通知回應為止。 請確定重試邏輯已指定逾時，以便API呼叫不會封鎖並導致效能延遲。
