---
title: 使用.NET SDK傳送顯示或按一下通知給 [!DNL Adobe Target]
description: 瞭解如何使用sendNotifications()將顯示通知或點選通知傳送至 [!DNL Adobe Target] 以進行測量和報告。
feature: APIs/SDKs
exl-id: 724e787c-af53-4152-8b20-136f7b5452e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# 傳送通知(.NET)

## 說明

`SendNotifications()`是用來傳送顯示或點選通知給[!DNL Adobe Target]以進行測量和報告。

>[!NOTE]
>
>當具有必要引數的`Execute`物件位於請求本身中時，曝光將自動遞增以符合活動資格。

會自動增加曝光次數的SDK方法為：

* `GetOffers()`
* `GetAttributes()`

當在要求中傳遞`Prefetch`物件時，曝光次數不會自動遞增到具有`Prefetch`物件中mbox的活動。 `SendNotifications()`必須用於預先擷取的體驗，以增加曝光次數和轉換次數。

## 方法

### 建立

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.SendNotifications(TargetDeliveryRequest request)
```

## 範例

首先，讓我們建置[!UICONTROL Target Delivery API]要求，以預先擷取`home`和`product1` mbox的內容。

### \.NET

```dotnet {line-numbers="true"}
var mboxRequests = new List<MboxRequest>
    {
        new (index: 1, name: "home"),
        new (index: 2, name: "product1"),
    };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetPrefetch(new PrefetchRequest(mboxes: mboxRequests))
    .Build();

// Next, we fetch the offers via Target .NET SDK GetOffers() API
var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```

成功的回應將包含[!DNL Target Delivery API]回應物件，其中包含要求mbox的預先擷取內容。 範例`targetResponse.Response`物件可能顯示如下：

### \.NET

```dotnet {line-numbers="true"}
{
  "status": 200,
  "requestId": "e8ac2dbf5f7d4a9f9280f6071f24a01e",
  "id": {
    "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "adobetargetmobile",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "home",
        "options": [
          {
            "type": "html",
            "content": "HOME OFFER",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      },
      {
        "index": 1,
        "name": "product1",
        "options": [
          {
            "type": "html",
            "content": "TEST OFFER 1",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      }
    ]
  }
}
```

記下每個[!DNL Target]內容選項中的`mbox`名稱和`state`欄位，以及`eventToken`欄位。 每個內容選項一顯示，即應在`SendNotifications()`請求中提供這些選項。 假設`product1` mbox已顯示在非瀏覽器裝置上。 通知要求會顯示如下：

### \.NET

```dotnet {line-numbers="true"}
var mboxNotifications = new List<Notification>
{
    new (id: "1", type: MetricType.Display, timestamp: DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
        mbox: new NotificationMbox("product1", "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"),
        tokens: new List<string> { "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" })
}; 

var mboxNotificationRequest = new TargetDeliveryRequest.Builder()
    .SetNotifications(mboxNotifications)
    .Build();
```

請注意，我們已在預先擷取回應中納入對應至已傳遞[!DNL Target]選件的mbox狀態和事件權杖。 建立通知要求後，我們可以透過`SendNotifications()` API方法將其傳送至[!DNL Target]：

### \.NET

```dotnet {line-numbers="true"}
var notificationResponse = targetClient.SendNotifications(mboxNotificationRequest);
```
