---
title: 如何在中使用非同步請求 [!DNL Adobe Target] Python SDK
description: 瞭解如何 [!DNL Target] Python SDK支援非同步要求，因此可將有效目標時間減少為零。
feature: APIs/SDKs
exl-id: fafb9e28-5ac5-41c1-8e7f-f40550b6749f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 14%

---

# 取得屬性(Python)

## 說明

`get_attributes()` 用於擷取實驗和個人化體驗 [!DNL Target] 和擷取屬性值。


## 方法

### getAttributes

```python {line-numbers="true"}
target_client_instance.get_attributes(mbox_names, options)
```

## 參數

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| mbox名稱(_N) | 清單[str] | 是 | 無 | mbox名稱清單 |
| options | dict | 否 | 無 | 與使用的選項相同 [取得優惠方案](get-offers.md) |

## 屬性提供者

此 `AttributesProvider` 傳回者 `target_client.get_attributes()` 有以下方法：

| 方法 | 傳回類型 | 說明 |
| --- | --- | --- |
| get_value(mbox_name， key) | any | 傳回指定mbox名稱和屬性索引鍵的值 |
| as_object(mbox_name) | dict | 傳回具有索引鍵值配對的簡單json物件 |
| get_response() | [TargetDeliveryResponse](https://github.com/adobe/target-python-sdk/blob/main/target_python_sdk/types/target_delivery_response.py) | 傳回通常由傳回的回應物件 `get_offers` |

## 範例

### Python

```python {line-numbers="true"}
def client_ready_callback():
    context = Context(channel=ChannelType.WEB)
    mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_attributes_options = {
      "request": delivery_request
    }

    attributes_provider = target_client.get_attributes(["demo-engineering-flags"], get_attributes_options)
    # returns just the value of searchProviderId from the demo-engineering-flags mbox offer
    search_provider_id = attributes_provider.get_value("demo-engineering-flags", "searchProviderId")

    # returns a simple dict object representing the demo-engineering-flags mbox offer
    engineering_flags = attributes_provider.as_object("demo-engineering-flags")
    """  the value of engineeringFlags looks like this
        {
            "cdnHostname": "cdn.cloud.corp.net",
            "searchProviderId": 143,
            "hasLegacyAccess": false
        }
    """

    asset_url = "http://{}/path/to/asset".format(engineering_flags.get("cdnHostname"))


client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
