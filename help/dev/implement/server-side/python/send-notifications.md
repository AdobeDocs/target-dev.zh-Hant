---
title: '使用Python SDK傳送顯示或按一下通知給 [!DNL Adobe Target] '
description: 瞭解如何使用sendNotifications()將顯示通知或點選通知傳送至 [!DNL Adobe Target] 以進行測量和報告。
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 8%

---

# 傳送通知(Python)

## 說明

`send_notifications()`是用來傳送顯示或點選通知給[!DNL Adobe Target]以進行測量和報告。

>[!NOTE]
>
>當具有必要引數的`execute`物件位於請求本身中時，曝光將自動遞增以符合活動資格。

會自動增加曝光次數的SDK方法如下：

* `get_offers()`
* `get_attributes()`

當在要求中傳遞`prefetch`物件時，曝光次數不會自動遞增到具有`prefetch`物件中mbox的活動。 `Send_notifications()`必須用於預先擷取的體驗，以增加曝光次數和轉換次數。

## 方法

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## 參數

`options`具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 請求 | DeliveryRequest | 是 | 無 | 符合[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)要求 |
| target_cookie | str | no | 無 | [!DNL Target] Cookie |
| target_location_hint | str | no | 無 | [!DNL Target]位置提示 |
| consumer_id | str | no | 無 | 拼接多個呼叫時，應提供不同的消費者ID |
| customer_ids | 清單[CustomerId] | no | 無 | VisitorId相容格式的客戶ID清單 |
| session_id | str | no | 無 | 用於連結多個請求 |
| callBack | 可呼叫 | no | 無 | 如果以非同步方式處理請求，則會在回應就緒時叫用回呼 |
| err_callback | 可呼叫 | no | 無 | 如果以非同步方式處理請求，則會在引發例外狀況時叫用錯誤回呼 |

## 傳回值

`Returns`同步呼叫（預設）時為`TargetDeliveryResponse`，或呼叫回撥時為`AsyncResult`。 `TargetDeliveryResponse`具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 回應 | DeliveryResponse | 符合[[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)回應 |
| target_cookie | dict | [!DNL Target] Cookie |
| target_location_hint_cookie | dict | [!DNL Target]位置提示Cookie |
| analytics_details | 清單[AnalyticsResponse] | 使用者端[!DNL Analytics]使用狀況下的[!DNL Analytics]承載 |
| trace | 清單[dict] | 所有請求mbox/檢視的彙總追蹤資料 |
| response_tokens | 清單[dict] | [回應&#x200B;權杖](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)的清單 |
| meta | dict | 用於裝置上決策的其他決策中繼資料 |

## 範例

首先，讓我們建置[!UICONTROL Target Delivery API]要求，以預先擷取`home`和`product1` mbox的內容。

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

成功的回應將包含[!UICONTROL Target Delivery API]回應物件，其中包含要求mbox的預先擷取內容。 範例`target_response["response"]`物件（格式為dict）可能如下所示：

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

記下每個Target內容選項中的mbox `name`和`state`欄位，以及`eventToken`欄位。 每個內容選項一顯示，即應在`send_notifications()`請求中提供這些選項。 假設`product1` mbox已顯示在非瀏覽器裝置上。 通知要求會顯示如下：

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

請注意，我們已在預先擷取回應中納入對應至已傳遞[!DNL Target]選件的mbox狀態和事件權杖。 建立通知要求後，我們可以透過[!DNL Target] API方法將其傳送至`send_notifications()`：

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
