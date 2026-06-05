---
title: '使用Java SDK傳送顯示或按一下通知給 [!DNL Adobe Target] '
description: 瞭解如何使用sendNotifications()將顯示通知或點選通知傳送至 [!DNL Adobe Target] 以進行測量和報告。
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
TQID: https://experienceleague.adobe.com/aoa6x9BkuaC-6XaqU03mvWXqWucNSF9eJAk41Bk0-nE
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 232
ht-degree: 2%

---

# 傳送通知(Java)

## 說明

`sendNotifications()`是用來傳送顯示或點選通知給[!DNL Adobe Target]以進行測量和報告。

>[!NOTE]
>
>當具有必要引數的`execute`物件位於請求本身中時，曝光將自動遞增以符合活動資格。

會自動增加曝光次數的SDK方法如下：

* `getOffers()`
* `getAttributes()`

當在要求中傳遞`prefetch`物件時，曝光次數不會自動遞增到具有`prefetch`物件中mbox的活動。 `sendNotifications()`必須用於預先擷取的體驗，以增加曝光次數和轉換次數。

## 方法

### 建立

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## 範例

首先，讓我們建置[!DNL Target Delivery API]要求，以預先擷取`home`和`product1` mbox的內容。

### 預先擷取

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

成功的回應將包含[!UICONTROL Target傳送API]回應物件，其中包含已要求mbox的預先擷取內容。 範例`targetResponse.response`物件可能如下所示：

### 回應

```javascript {line-numbers="true"}
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

記下[!DNL Target]內容選項中每個的mbox `name`和`state`欄位，以及`eventToken`欄位。 每個內容選項一顯示，即應在`sendNotifications()`請求中提供這些選項。 假設`product1` mbox已顯示在非瀏覽器裝置上。 通知要求如下：

### 請求

```javascript {line-numbers="true"}
TargetDeliveryRequest mboxNotificationRequest = TargetDeliveryRequest.builder().notifications(new ArrayList() {{
    add(new Notification()
            .id("1")
            .timestamp(System.currentTimeMillis())
            .type(MetricType.DISPLAY)
            .mbox(new NotificationMbox()
                    .name("product1")
                    .state("J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
            )
            .tokens(Arrays.asList(new String[]{"t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="}))
    );
}}).build();
```

請注意，我們已在預先擷取回應中納入與已傳遞之[!DNL Target]選件相對應的mbox狀態和事件權杖。 建立通知要求後，我們可以透過`sendNotifications()` API方法將其傳送至[!DNL Target]：

### 回應

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
