---
title: Adobe Target設定檔API
description: 瞭解如何使用Adobe Target設定檔API將訪客資料傳送至 [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
source-git-commit: 9707680ddcf0c373c635aa9f3cb5ba1b74cf90a3
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 1%

---

# [!DNL Adobe Target Profiles API] 概述

[!DNL Adobe Target] 為每個個別使用者建立和維護設定檔。 此設定檔儲存在 [!DNL Target] 邊緣叢集，並在每次造訪後即時更新。

## 設定檔 [!DNL Postman] 集合

[!DNL Postman] 是可讓您輕鬆引發API呼叫的應用程式。 這個 [!DNL Postman] 集合包含所有 [!DNL Profile API] 呼叫。 按一下 [在Postman中執行](https://www.getpostman.com/collections/ec7376f9028977ccaa99){target=_blank} 以匯入設定檔API集合。

## 的結構 [!DNL Target] 設定檔

Target設定檔包含下列物件：

| 物件 | 詳細資料 |
| --- | --- |
| `clientcode` | 此 [!DNL Target] 與設定檔相關聯的使用者端代碼。 |
| `visitorId` | 設定檔的識別碼。 這可以是 `tntid`， `thirdpartyid`，或 `marketingcloudvisitorid`. |
| `modifiedAt` | 設定檔上次更新的時間戳記。 |
| `profileAttributes` | 該個別設定檔上所有儲存為索引鍵/值組的設定檔屬性清單。 |

### 範例設定檔結構

```
{
    "client": "<your-tenant-name>",
    "visitorId": "a1-mbox3rdPartyId",
    "modifiedAt": "2017-08-18T17:53:39.003-04:00",
    "profileAttributes": {
        "insurance": {
            "value": "false",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "country": {
            "value": "france",
            "modifiedAt": "2017-07-31T14:26:30.879-04:00"
        },
        "checking": {
            "value": "true",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "user.memberlevel": {
            "value": "0.0",
            "modifiedAt": "2017-08-09T18:18:04.661-04:00"
        },
        "param1": {
            "value": "value1",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "param2": {
            "value": "value2",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "firstSessionStart": {
            "value": "1500648715286",
            "modifiedAt": "2017-07-21T10:51:55.286-04:00"
        },
        "entity.name": {
            "value": "my-entityName",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "entity.id": {
            "value": "my-entityId",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        }
    }
}
```
