---
title: 傳送顯示或按一下通知至 [!DNL Adobe Target] 使用Python SDK
description: 瞭解如何使用sendNotifications()傳送顯示通知或按一下通知至 [!DNL Adobe Target] 用於測量和報表。
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 8%

---

# 傳送通知(Python)

## 說明

`send_notifications()` 用於傳送顯示或點按通知至 [!DNL Adobe Target] 用於測量和報表。

>[!NOTE]
>
>當 `execute` 具有必要引數的物件位於請求本身內，曝光將自動遞增以符合活動資格。

會自動增加曝光次數的SDK方法為：

* `get_offers()`
* `get_attributes()`

當 `prefetch` 物件是在要求內傳遞，曝光次數不會針對內有mbox的活動自動遞增 `prefetch` 物件。 `Send_notifications()` 必須用於預先擷取的體驗，以增加曝光次數和轉換次數。

## 方法

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## 參數

`options` 具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 請求 | DeliveryRequest | 是 | 無 | 符合 [[!UICONTROL Target傳送API]](/help/dev/implement/delivery-api/overview.md) 請求 |
| target_cookie | str | no | 無 | [!DNL Target] Cookie |
| target_location_hint | str | no | 無 | [!DNL Target] 位置點擊 |
| consumer_id | str | no | 無 | 拼接多個呼叫時，應提供不同的消費者ID |
| customer_ids | 清單[CustomerId] | no | 無 | VisitorId相容格式的客戶ID清單 |
| session_id | str | no | 無 | 用於連結多個請求 |
| callBack | 可呼叫 | no | 無 | 如果以非同步方式處理請求，則會在回應就緒時叫用回呼 |
| err_callback | 可呼叫 | no | 無 | 如果以非同步方式處理請求，則會在引發例外狀況時叫用錯誤回呼 |

## 傳回

`Returns` a `TargetDeliveryResponse` 如果同步呼叫（預設），或是 `AsyncResult` 若以回呼呼叫。 `TargetDeliveryResponse` 具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 回應 | DeliveryResponse | 符合 [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) 回應 |
| target_cookie | dict | [!DNL Target] Cookie |
| target_location_hint_cookie | dict | [!DNL Target] 位置提示Cookie |
| analytics_details | 清單[AnalyticsResponse] | [!DNL Analytics] 裝載，若是使用者端 [!DNL Analytics] 使用狀況 |
| trace |  | 清單[dict] | 所有請求mbox/檢視的彙總追蹤資料 |
| response_tokens | 清單[dict] | 清單 [回&#x200B;應Token](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | dict | 用於裝置上決策的其他決策中繼資料 |

## 範例

首先，讓我們建立 [!UICONTROL Target傳送API] 要求預先擷取的內容 `home` 和 `product1` mbox。

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

成功的回應將包含 [!UICONTROL Target傳送API] 回應物件，其中包含所要求mbox的預先擷取內容。 範例 `target_response["response"]` 物件（格式化為指令）可能顯示如下：

### Python

```python {line-numbers="true"}
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

記下mbox `name` 和 `state` 欄位，以及 `eventToken` 欄位，位於每個Target內容選項中。 這些應於以下位置提供： `send_notifications()` 每個內容選項一顯示，立即要求。 假設 `product1` mbox已顯示在非瀏覽器裝置上。 通知要求會顯示如下：

### Python

```python {line-numbers="true"}
notification_mbox = NotificationMbox(name="product1",
                                     state="J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
notification = Notification(
    id="1",
    type=MetricType.DISPLAY,
    timestamp=1621530726000,  # Epoch time in milliseconds
    mbox=notification_mbox,
    tokens=["t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="]
)
notification_request = DeliveryRequest(notifications=[notification])
```

請注意，我們已納入與對應的mbox狀態和事件權杖 [!DNL Target] 在預先擷取回應中傳遞的選件。 建立通知請求後，我們可以將其傳送到 [!DNL Target] 透過 `send_notifications()` API方法：

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
