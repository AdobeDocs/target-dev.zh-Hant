---
title: Adobe Target Delivery API快速入門
description: 如何使用 [!UICONTROL Adobe Target傳送API]？
keywords: 傳送api
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# 開始使用 [!UICONTROL Adobe Target傳送API]

A [!UICONTROL Target傳送API] 呼叫看起來像這樣：

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

此 `clientCode` 可於下列位置擷取： [!DNL Target] UI，瀏覽至 **[!UICONTROL 管理]** > **[!UICONTROL 實施]**.

進行之前 [!UICONTROL Target傳送API] 呼叫，請依照下列步驟操作，以確保回應包含可顯示使用者的相關體驗：

1. 建立 [!DNL Target] 活動(A/B、XT、AP或Recommendations) [表單式撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en) 或 [視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html).
1. 使用傳送API針對 [!DNL Target] 在步驟2中建立的活動。
1. 向訪客呈現體驗。

## Postman集合 {#postman}

Postman是應用程式，可讓您輕鬆引發API呼叫。 [此Postman集合](https://run.pstmn.io/button.svg) 包含傳送API呼叫的範例。
