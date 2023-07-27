---
title: 訂閱中的事件 [!DNL Adobe Target] Python SDK
description: 瞭解如何使用訂閱Python SDK中發生的各種事件。 [!UICONTROL OnDeviceDecisioningHandler] 物件。
feature: APIs/SDKs
exl-id: 4e32e3b5-6072-4703-b09d-abb467aa1304
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 3%

---

# SDK事件(Python)

## 說明

時間 [初始化SDK](initialize-sdk.md)，則 `options["events"]` dict是選用的物件，具有事件名稱索引鍵和回呼函式值。 可用來訂閱SDK內發生的各種事件。 例如， `client_ready` 事件可與回呼函式搭配使用，當SDK準備好進行方法呼叫時，系統會叫用該回呼函式。

當 `callback` 函式中，傳入事件物件。 每個事件都有一個 `type` 和事件名稱相對應，某些事件則包含其他屬性以及相關資訊。

## 事件

| 事件名稱（型別） | 說明 | 其他事件屬性 |
| --- | --- | --- |
| client_ready | 已在下載成品且SDK已準備好進行get_offers呼叫時發出。 建議使用時 | 裝置上決策方法。 | 無 |
| artifact_download_succeeded | 每次下載新成品時發出。 | artifact_payload， artifact_location |
| artifact_download_failed | 每當成品無法下載時發出。 | artifact_location，錯誤 |

## 範例

### Python

```python {line-numbers="true"}
def client_ready_callback():
    # make get_offers requests

def artifact_download_succeeded(event):
    print("The artifact was successfully downloaded from {}".format(event.artifact_location))
    # optionally do something with event.artifact_payload, like persist it

def artifact_download_failed(event):
    print("The artifact failed to download from {} with the following error: {}"
          .format(event.artifact_location, str(event.error)))

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback,
        "artifact_download_succeeded": artifact_download_succeeded,
        "artifact_download_failed": artifact_download_failed
    }
}
target_client = target_client.create(client_options)
```
