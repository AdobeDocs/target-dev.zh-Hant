---
title: Adobe Target Delivery API快速入門
description: 如何使用[!UICONTROL Adobe Target Delivery API]？
keywords: 傳送api
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/DC-YVq6VfAaqMU1utmIMw73gzp4PIJgQjaS0a8FQEO4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 132
ht-degree: 0%

---

# 開始使用[!UICONTROL Adobe Target Delivery API]

[!UICONTROL Target傳送API]呼叫看起來像這樣：

```
curl -X POST \
  'https://`clientCode`.tt.omtrdc.net/rest/v1/delivery?client=`clientCode`&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
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

您可以導覽至&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]**，從[!DNL Target] UI擷取`clientCode`。

進行[!UICONTROL Target傳送API]呼叫之前，請依照下列步驟操作，以確保回應包含可顯示一般使用者的相關體驗：

1. 使用[表單式撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=zh-Hant)或[視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=zh-Hant)建立[!DNL Target]活動（A/B、XT、AP或Recommendations）。
1. 使用傳送API針對在步驟2中建立的[!DNL Target]活動中使用的mbox取得回應。
1. 向訪客呈現體驗。
