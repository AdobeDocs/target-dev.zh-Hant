---
title: Adobe Target Delivery API使用者許可權
description: Adobe Target Delivery API使用者許可權
badgePremium: label="Premium" type="Positive" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=en#premium newtab=true" tooltip="檢視Target Premium包含的內容。"
keywords: 傳送api
exl-id: 332f90bd-4079-4653-aa38-b35837631c94
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# 使用者許可權(Premium)

[!DNL Adobe] 可讓客戶在使用Adobe Target時管理其使用者的許可權。 為了成功 [!UICONTROL Adobe Target傳送API] 呼叫，具有正確許可權的Token必須在API呼叫中傳遞。 為了進一步瞭解使用者許可權以及如何擷取權杖造訪 [本檔案](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
      "context": {
        "channel": "web",
        "browser" : {
          "host" : "demo"
        },
        "address" : {
          "url" : "http://demo.dev.tt-demo.com/demo/store/index.html"
        },
        "screen" : {
          "width" : 1200,
          "height": 1400
        }
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
            "index" : 1
          }
        ]
      }
    }'
```

擁有對應的Token後，請將其傳遞至 `property` -> `token` 執行每個API呼叫時。 如果 `property` -> `token` 並未在每個API呼叫中傳遞，您將不會取得任何 `content` 從Adobe Target回來。

```
{
    "status": 200,
    "requestId": "07ce783d-58b9-461c-9f4c-6873aeb00c01",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage"
            }
        ]
    }
}
```

如上所示，無須傳遞 `property` -> `token`，您將無法取回任何內容。 如果您期望從API呼叫取得內容，但並未從回應中擷取任何內容，很可能是因為  `property` -> `token` 未提供，或傳遞時沒有正確的許可權。
