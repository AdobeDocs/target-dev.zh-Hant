---
title: 如何在中使用非同步請求 [!DNL Adobe Target] Python SDK
description: 瞭解如何 [!DNL Target] Python SDK支援非同步要求，因此可將有效目標時間減少為零。
feature: APIs/SDKs
exl-id: 44ab74e5-3c1a-49cf-9fff-fe523b0c2592
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 4%

---

# 非同步請求(Python)

## 說明

伺服器端整合的好處之一，就是您可以使用平行處理功能，在伺服器端利用龐大的頻寬和運算資源。 [!DNL Target] Python SDK支援非同步要求，因此可將有效目標時間減少為零。

## 支援的方法

### Python

```python {line-numbers="true"}
get_offers(options)
send_notifications(options)
get_attributes(mbox_names, options)
```

## 範例

使用 `asyncio` Python 3.9+中的模組非同步/等待可能如下所示：

### Python

```python {line-numbers="true"}
async def execute_mboxes(self, mboxes):
    context = Context(channel=ChannelType.WEB)
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }
    return await asyncio.to_thread(target_client.get_offers, get_offers_options)

async def get_target_delivery_response(mboxes):
    target_delivery_response = await execute_mboxes(mboxes)
    response = Response(target_delivery_response.get("response").to_str(), status=200, mimetype='application/json')
    return response

mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
return asyncio.run(get_target_delivery_response(mboxes)
```

此範例假設您使用Python 3.9+。 如果使用舊版Python，您仍然可以透過傳入來傳送非同步請求 `options.callback` 至 `get_offers`. 如需有關使用回呼或非同步/等待的非同步執行的詳細資訊，請參閱範例Flask應用程式。 [此處](https://github.com/adobe/target-python-sdk/blob/main/samples/app.py).
