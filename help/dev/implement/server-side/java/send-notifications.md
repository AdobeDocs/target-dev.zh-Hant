---
title: 傳送顯示或按一下通知至 [!DNL Adobe Target] 使用Java SDK
description: 瞭解如何使用sendNotifications()傳送顯示通知或按一下通知至 [!DNL Adobe Target] 用於測量和報表。
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 2%

---

# 傳送通知(Java)

## 說明

`sendNotifications()` 用於傳送顯示或點按通知至 [!DNL Adobe Target] 用於測量和報表。

>[!NOTE]
>
>當 `execute` 具有必要引數的物件位於請求本身內，曝光將自動遞增以符合活動資格。

會自動增加曝光次數的SDK方法為：

* `getOffers()`
* `getAttributes()`

當 `prefetch` 物件是在要求內傳遞，曝光次數不會針對內有mbox的活動自動遞增 `prefetch` 物件。 `sendNotifications()` 必須用於預先擷取的體驗，以增加曝光次數和轉換次數。

## 方法

### 建立

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## 範例

首先，讓我們建立 [!DNL Target Delivery API] 要求預先擷取的內容 `home` 和 `product1` mbox。

### 預先擷取

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

成功的回應將包含 [!UICONTROL Target傳送API] 回應物件，其中包含所要求mbox的預先擷取內容。 範例 `targetResponse.response` 物件可能如下所示：

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

記下mbox `name` 和 `state` 欄位，以及 `eventToken` 欄位，位於每個欄位 [!DNL Target] 內容選項。 這些應於以下位置提供： `sendNotifications()` 每個內容選項一顯示，立即要求。 假設 `product1` mbox已顯示在非瀏覽器裝置上。 通知要求如下：

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

請注意，我們已納入與對應的mbox狀態和事件權杖 [!DNL Target] 在預先擷取回應中傳遞的選件。 建立通知請求後，我們可以將其傳送到 [!DNL Target] via `sendNotifications()` API方法：

### 回應

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
