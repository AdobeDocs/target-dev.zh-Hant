---
title: Adobe Target Delivery API快速入門
description: 如何使用[!UICONTROL Adobe Target Delivery API]？
keywords: 傳送api
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5a1c38d448cb7446b7b26cd0dc882976ba94dd3
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 0%

---

# 開始使用[!UICONTROL Adobe Target Delivery API]

[!UICONTROL Target Delivery API]呼叫看起來像這樣：

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

您可以導覽至&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]**，從[!DNL Target] UI擷取`clientCode`。

執行[!UICONTROL Target Delivery API]呼叫之前，請依照下列步驟操作，以確保回應包含向一般使用者顯示的相關體驗：

1. 使用[表單式撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en)或[視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)建立[!DNL Target]活動(A/B、XT、AP或Recommendations)。
1. 使用傳送API針對在步驟2中建立的[!DNL Target]活動中使用的mbox取得回應。
1. 向訪客呈現體驗。
