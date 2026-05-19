---
title: 初始化 [!DNL Adobe Target] Python SDK以記錄要求
description: 瞭解如何在 [!DNL Adobe Target] Python SDK中記錄要求。
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
TQID: https://experienceleague.adobe.com/9LSln8V3QIG9GTok2yTTnKvhlpQhaed3a-qJyA4jErg
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: a94ced60-8199-4549-b453-ede2acb4101e
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 97
ht-degree: 3%

---

# 記錄器(Python)

## 說明

當[初始化SDK](initialize-sdk.md)時，`options["logger"]`物件是選用物件。 根據預設，將在`adobe.target`名稱空間下建立INFO層級記錄器。 但是，為了在問題發生時有效地自訂記錄層級或偵錯，可以在初始化SDK時提供`logger`物件。

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
