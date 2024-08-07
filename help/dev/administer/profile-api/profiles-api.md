---
title: Adobe Target設定檔API
description: 瞭解如何使用Adobe Target設定檔API將訪客資料傳送至 [!DNL Target]。
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 480cbbbe-4822-48c3-80d4-53628dee57b0
source-git-commit: e2462d12cf58ab5a588c13a96df5e6abafb9d675
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 1%

---

# [!DNL Adobe Target Profiles API]總覽

[!DNL Adobe Target]會為每個個別使用者建立和維護設定檔。 此設定檔儲存在[!DNL Target]邊緣叢集上，並在每次造訪後即時更新。

## [!DNL Target]設定檔的結構

Target設定檔包含下列物件：

| 物件 | 詳細資料 |
| --- | --- |
| `clientcode` | 與設定檔相關聯的[!DNL Target]使用者端代碼。 |
| `visitorId` | 設定檔的識別碼。 這可以是`tntid`、`thirdpartyid`或`marketingcloudvisitorid`。 |
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
