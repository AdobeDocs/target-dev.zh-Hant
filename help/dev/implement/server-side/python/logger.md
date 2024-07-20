---
title: 初始化 [!DNL Adobe Target] Python SDK以記錄要求
description: 瞭解如何在 [!DNL Adobe Target] Python SDK中記錄請求。
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 3%

---

# 記錄器(Python)

## 說明

當[初始化SDK](initialize-sdk.md)時，`options["logger"]`物件是選用物件。 根據預設，將在`adobe.target`名稱空間下建立INFO層級記錄器。 不過，為了在發生問題時有效地自訂記錄層級或偵錯，可以在初始化SDK時提供`logger`物件。

`logger`物件必須有`debug()`和`error()`方法。 當提供適當的記錄器時，將會記錄[!DNL Target]個要求與回應。

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
