---
title: 使用create方法初始化Python SDK
description: 瞭解如何使用create方法初始化Python SDK並將[!UICONTROL TargetClient]例項化，以呼叫 [!DNL Adobe Target] 進行實驗與個人化體驗。
feature: APIs/SDKs
exl-id: 3e231e8e-696d-45c7-b733-79bf99da5bec
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 17%

---

# 初始化Python SDK

說明
使用`create`方法，以初始化Python SDK並將[!UICONTROL Target Client]例項化，以呼叫[!DNL Adobe Target]進行實驗與個人化體驗。

## 方法

### 建立

```python {line-numbers="true"}
TargetClient.create(options)
```

## 參數

`options`具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 使用者端 | str | 是 | 無 | [!UICONTROL Adobe Target client ID] |
| organization_id | str | 是 | 無 | [!UICONTROL Experience Cloud Organization ID] |
| timeout | int | 否 | 3000 | 逾時（毫秒） |
| server_domain | str | 否 | `client.tt.omtrdc.net` |  | 覆寫預設主機名稱 |
| secure | 布林值 | 否 | true | 取消設定以強制執行HTTP配置 |
| logger | 物件 | 否 | 資訊記錄器 |  | 取代預設的INFO記錄器 |
| target_location_hint | str | 否 | 無 | [!DNL Target]位置提示 |
| property_token | str | 否 | 無 | [!DNL Target]屬性權杖。 若在此處指定，所有get_offers呼叫都會使用此值。 |
| decisioning_method | str | 否 | 伺服器端 | 決定要使用的決策方法（[裝置上](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、伺服器端、混合式） |
| polling_interval | int | 否 | 300000 （5分鐘） | [裝置上決策規則成品](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)的輪詢間隔（以毫秒為單位） |
| artifact_location | str | 否 | 無 | [裝置上決策規則成品](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)的完整URL。 覆寫內部決定的位置。 |
| artifact_payload | 物件 | 否 | 無 | [裝置上決策規則成品](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)的JSON裝載。 若指定，會加以使用，而非向URL要求。 |
| [events](sdk-events.md) | dict &lt;str， callable> | 否 | 無 | 具有事件名稱索引鍵和回呼函式值的選用物件 |
| environment_id | int | 否 | 生產 | [!DNL Target]環境識別碼 |
| 環境 | str | 否 | 生產 | [!DNL Target]環境名稱 |
| cdn_environment | str | 否 | 生產 | CDN環境名稱 |
| telemetry_enabled | 布林值 | 否 | True | 如果設為False，將不會傳送遙測資料給[!DNL Adobe] |
| version | str | 否 | 無 | 此SDK的版本號碼 |

## 範例

### Python

```python {line-numbers="true"}
from target_python_sdk import TargetClient

def client_ready_callback(ready_event):
    # make calls to Adobe Target

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
