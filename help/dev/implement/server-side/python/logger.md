---
title: 初始化 [!DNL Adobe Target] 用於記錄請求的Python SDK
description: 瞭解如何在中記錄請求 [!DNL Adobe Target] Python SDK.
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 3%

---

# 記錄器(Python)

## 說明

時間 [初始化SDK](initialize-sdk.md)，則 `options["logger"]` 物件是選用的物件。 根據預設，系統會在底下建立INFO層級記錄器 `adobe.target` 名稱空間。 但是，為了在發生問題時有效地自訂記錄層級或偵錯， `logger` 初始化SDK時可提供物件。

此 `logger` 物件必須具有 `debug()` 和 `error()` 方法。 提供適當的記錄器時， [!DNL Target] 將記錄請求和回應。

## 範例

### Python

```python {line-numbers="true"}
logger = logging.getLogger("org.logger")
logger.setLevel(logging.DEBUG)

client_options = {
  "client": "acmeclient",
  "organization_id": "1234567890@AdobeOrg",
  "logger": logger
}
target_client = TargetClient.create(client_options)
```

您應該會看到主控台中列印的請求和回應。
