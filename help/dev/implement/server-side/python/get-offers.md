---
title: 使用Python SDK時，請在 [!DNL Adobe Target] 中使用getOffers()
description: 瞭解如何使用getOffers()執行決定並從 [!DNL Adobe Target]擷取體驗。
feature: APIs/SDKs
exl-id: 9539b806-e070-430e-80cf-cf632ce3f207
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 12%

---

# 取得選件(Python)

## 說明

`get_offers()`用於執行決定並從[!DNL Adobe Target]擷取體驗。


## 方法

### getOffers

```python {line-numbers="true"}
target_client_instance.get_offers(options)
```

## 參數

`options` dict具有下列結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 請求 | DeliveryRequest | 是 | 無 | 符合[[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)要求 |
| target_cookie | str | no | 無 | [!DNL Target] Cookie |
| target_location_hint | str | no | 無 | [!DNL Target]位置提示 |
| consumer_id | str | no | 無 | 拼接多個呼叫時，應提供不同的消費者ID |
| customer_ids | 清單[CustomerId] | no | 無 | VisitorId相容格式的客戶ID清單 |
| session_id | str | no | 無 | 用於連結多個請求 |
| callBack | 可呼叫 | no | 無 | 如果以非同步方式處理請求，則會在回應就緒時叫用回呼 |
| err_callback | 可呼叫 | no | 無 | 如果以非同步方式處理請求，則會在引發例外狀況時叫用錯誤回呼 |

## 傳回值

如果同步呼叫（預設），則傳回`TargetDeliveryResponse`，如果以回撥呼叫，則傳回`AsyncResult`。 `TargetDeliveryResponse`具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 回應 | DeliveryResponse | 符合[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)回應 |
| target_cookie | dict | [!DNL Target] Cookie |
| target_location_hint_cookie | dict | [!DNL Target]位置提示Cookie |
| analytics_details | 清單[AnalyticsResponse] | 使用者端Analytics使用狀況下的Analytics裝載 |
| trace | 清單[dict] | 所有請求mbox/檢視的彙總追蹤資料 |
| response_tokens | 清單[dict] | [回應Token&#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=zh-Hant)的清單 |
| meta | dict | 用於裝置上決策的其他決策中繼資料 |

用來將資料傳回瀏覽器的`target_cookie`和`target_location_hint_cookie`物件具有以下結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| 名稱 | str | Cookie 名稱 |
| value | any | Cookie值，該值將會轉換為字串 |
| max_age | int | `max_age option`方便您設定相對於目前時間（以秒為單位）的過期時間 |

用於表示目標回應狀態的`meta`物件具有下列結構：

| 名稱 | 類型 | 說明 |
| --- | --- | --- |
| decisioning_method | str | 使用的決策方法：裝置上或伺服器端 |
| remote_mboxes | 清單`[str]` | 當決定方法為`on-device`時，會提供無法完全決定裝置上的mbox名稱陣列。 換句話說，需要[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)要求。 |
| remote_views | 清單`[str]` | 當決定方法為裝置上時，會提供無法完全決定裝置上的檢視名稱陣列。 換句話說，需要[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)要求。 |

## 範例

### Python

```python {line-numbers="true"}
def client_ready_callback():
    context = Context(channel=ChannelType.WEB)
    mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }

    target_delivery_response = target_client.get_offers(get_offers_options)


client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
