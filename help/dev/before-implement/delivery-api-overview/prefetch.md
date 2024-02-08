---
title: Adobe Target傳送API預先擷取
description: 如何在中使用預先擷取 [!UICONTROL Adobe Target傳送API]？
keywords: 傳送api
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
source-git-commit: 901b56a91c69c9c5a2bd322aa999d45c47058a5e
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---

# 預先擷取

預先擷取可讓行動應用程式和伺服器等使用者端透過一次請求，為多個mbox或檢視擷取內容、在本地快取，並在稍後通知 [!DNL Target] 訪客造訪這些mbox或檢視時。

使用預先擷取時，請務必熟悉下列詞語：

| 欄位名稱 | 說明 |
| --- | --- |
| `prefetch` | 應擷取但不應標示為已瀏覽的mbox和檢視清單。 此 [!DNL Target] Edge傳回 `eventToken` 預先擷取陣列中存在的每個mbox或檢視。 |
| `notifications` | 先前預先擷取且應標示為已瀏覽的mbox和檢視清單。 |
| `eventToken` | 預先擷取內容時傳回的雜湊加密權杖。 此Token應傳回 [!DNL Target] 在 `notifications` 陣列。 |

## 預先擷取Mbox

使用者端（例如行動應用程式和伺服器）可在工作階段中，預先擷取特定訪客的多個mbox，並加以快取，以避免多次呼叫 [!UICONTROL Adobe Target傳送API].

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=7abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '
{
  "id": {
    "tntId": "abcdefghijkl00023.1_1"
  },
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
    "prefetch": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      },
      {
        "name" : "SummerShoesOffer",
        "index" : 2
      },
      {
        "name" : "SummerDressOffer",
        "index" : 3
      }      
    ]
  }
}'
```

在 `prefetch` 欄位，新增一或多個 `mboxes` 您想要在工作階段中，為訪客預先擷取至少一次。 預先擷取這些資料之後 `mboxes`，您會收到下列回應：

```
{
    "status": 200,
    "requestId": "5efee0d8-3779-4b12-a74e-e04848faf191",
    "client": "demo",
    "id": {
        "tntId": "abcdefghijkl00023.1_1"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "prefetch": {
        "mboxes": [
            {
                "index": 1,
                "name": "SummerOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            },
            {
                "index": 2,
                "name": "SummerShoesOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next shoe purchase</b></p>"
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            },
            {
                "index": 3,
                "name": "SummerDressOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next dress purchase</b></p>"
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            }
        ]
    }
}
```

在回應中，您會看到 `content` 包含要向特定訪客顯示的體驗的欄位 `mbox`. 若要讓訪客在工作階段中與您的網頁或行動應用程式互動並造訪 `mbox` 在應用程式的任何特定頁面上，可以從快取中提供體驗，而不是製作另一個 [!UICONTROL Adobe Target傳送API] 呼叫。 不過，當體驗從傳遞至訪客時 `mbox`， a `notification` 會透過傳送API呼叫傳送，以便進行曝光記錄。 這是因為 `prefetch` 會快取呼叫，這表示訪客在呼叫快取時未曾看到體驗。 `prefetch` 呼叫發生。 若要進一步瞭解 `notification` 程式，請參閱 [通知](notifications.md).

## 使用預先擷取mbox `clickTrack` 使用時的量度 [!UICONTROL 目標分析] (A4T)

[[!UICONTROL 目標的Adobe Analytics]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html){target=_blank} (A4T)是一種跨解決方案的整合，可讓您根據以下專案建立活動： [!DNL Analytics] 轉換量度和受眾區段。

下列程式碼片段是預先擷取包含下列專案的mbox所產生的回應： `clickTrack` 要通知的量度 [!DNL Analytics] 已點按優惠方案的時間：

```
{
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "<mboxName>",
        "options": [
           ...
        ],
        "metrics": [
          {
            "type": "click",
            "eventToken": "<eventToken>",
             "analytics": {
               "payload": {
                 "pe": "tnt",
                 "tnta": "..."
               }
             }
          },
          }
        ],
        "analytics": {
          "payload": {
            "pe": "tnt",
            "tnta": "347565:1:0|2,347565:1:0|1"
          }
        }
      }
    ]
  }
}
```

>[!NOTE]
>
>mbox的預先擷取包含 [!DNL Analytics] 僅限合格活動的裝載。 預先擷取尚未合格活動的成功量度會導致報告不一致。

## 預先擷取檢視

檢視可更順暢地支援單頁應用程式(SPA)和行動應用程式。 檢視可視為視覺元素的邏輯群組，這些元素共同構成SPA或行動體驗。 現在，VEC已透過傳送API建立AB和XT活動，並修改於 [SPA的檢視](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md) 現在可以預先擷取。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=a3e7368c62d944c0855d424cd7a03ab0' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
  },
  "context": {
    "channel": "web",
    "window": {
      "width": 1819,
      "height": 842
    },
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "prefetch": {
    "views": [{}]
  }
}'
```

上述範例呼叫將會預先擷取透過SPA VEC為AB和XT活動建立的所有檢視，以便針對網頁顯示 `channel`. 在呼叫中請注意，我們要從訪客所在的AB或XT活動中預先擷取所有檢視 `tntId`：`84e8d0e211054f18af365d65f45e902b.28_131` 正在造訪 `url`：`https://target.enablementadobe.com/react/demo/#/` 符合「 」資格。

```
{
    "status": 200,
    "requestId": "14ce028e-d2d2-4504-b3da-32740fa8dd61",
    "client": "demo",
    "id": {
        "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "prefetch": {
        "views": [
            {
                "id": 228,
                "name": "checkout-express",
                "key": "checkout-express",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setHtml",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > FORM.col-md-4:eq(0) > DIV:nth-of-type(1) > DIV.mb-3:eq(2)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(1) > FORM:nth-of-type(2) > DIV:nth-of-type(1) > DIV:nth-of-type(3)",
                                "content": "<span style=\"color:#000080;\"><strong>*We charge an additional fee of $12.34 for faster delivery. If you choose express delivery get 15% off on your next order.</strong></span>"
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            },
            {
                "id": 5,
                "name": "home",
                "key": "home",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setHtml",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(1) > DIV.heading:eq(0) > H1.title:eq(0)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > H1:nth-of-type(1)",
                                "content": "<span style=\"color:#800000;\"><strong>Trending Items</strong></span>"
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            },
            {
                "id": 6,
                "name": "products",
                "key": "products",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setStyle",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > DIV.heading:eq(0) > BUTTON.btn:eq(0)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > BUTTON:nth-of-type(1)",
                                "content": {
                                    "background-color": "rgba(191,0,0,1)",
                                    "priority": "important"
                                }
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            }
        ]
    }
}
```

在 `content` 回應的欄位，備註中繼資料，例如 `type`， `selector`， `cssSelector`、和 `content`，可在使用者造訪您的頁面時，用來呈現體驗給一般使用者。 請注意 `prefetched` 如有必要，可以快取內容並轉譯給使用者。
