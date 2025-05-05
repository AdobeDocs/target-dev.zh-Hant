---
title: Adobe Target傳送API預先擷取
description: 如何在[!UICONTROL Adobe Target Delivery API]中使用預先擷取？
keywords: 傳送api
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
source-git-commit: 4ff2746b8b485fe3d845337f06b5b0c1c8d411ad
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 0%

---

# 預先擷取

預先擷取可讓行動應用程式和伺服器等使用者端在一個要求中擷取多個mbox或檢視的內容、在本地快取，並在訪客造訪這些mbox或檢視時通知[!DNL Target]。

使用預先擷取時，請務必熟悉下列詞語：

| 欄位名稱 | 說明 |
| --- | --- |
| `prefetch` | 應擷取但不應標示為已瀏覽的mbox和檢視清單。 [!DNL Target] Edge會針對預先擷取陣列中存在的每個mbox或檢視傳回`eventToken`。 |
| `notifications` | 先前預先擷取且應標示為已瀏覽的mbox和檢視清單。 |
| `eventToken` | 預先擷取內容時傳回的雜湊加密權杖。 此Token應傳回`notifications`陣列中的[!DNL Target]。 |

## 預先擷取Mbox

使用者端（例如行動應用程式和伺服器）可以在一個工作階段中，預先擷取特定訪客的多個mbox，並加以快取，以避免對[!UICONTROL Adobe Target Delivery API]發出多次呼叫。

```shell shell-session
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

在`prefetch`欄位中，針對工作階段中的訪客，新增一或多個您想要預先擷取至少一次的`mboxes`。 預先擷取這些`mboxes`後，您會收到下列回應：

```JSON {line-numbers="true"}
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

在回應中，您會看到包含要向訪客顯示特定`mbox`之體驗的`content`欄位。 快取到您的伺服器上時，這非常有用，這樣當訪客在工作階段中與您的網頁或行動應用程式互動，並在您應用程式的任何特定頁面上造訪`mbox`時，可以從快取中提供體驗，而不是進行另一個[!UICONTROL Adobe Target Delivery API]呼叫。 不過，當體驗從`mbox`傳遞至訪客時，會透過傳送API呼叫傳送`notification`，以進行曝光記錄。 這是因為已快取`prefetch`個呼叫的回應，這表示訪客在發生`prefetch`呼叫時尚未看到體驗。 若要進一步瞭解`notification`程式，請參閱[通知](notifications.md)。

## 使用[!UICONTROL Analytics for Target] (A4T)時具有`clickTrack`個量度的預先擷取mbox

[[!UICONTROL Adobe Analytics for Target]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=zh-Hant){target=_blank} (A4T)是跨解決方案的整合，可讓您根據[!DNL Analytics]轉換量度和受眾區段來建立活動。

下列程式碼片段是預先擷取包含`clickTrack`個量度的mbox的回應，以通知[!DNL Analytics]已點按選件：

```JSON {line-numbers="true"}
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
>mbox的預先擷取僅包含合格活動的[!DNL Analytics]裝載。 預先擷取尚未合格活動的成功量度會導致報告不一致。

## 預先擷取檢視

檢視可更順暢地支援單頁應用程式(SPA)和行動應用程式。 檢視可視為視覺元素的邏輯群組，這些元素共同構成SPA或行動體驗。 現在，透過傳送API，VEC建立的[[!UICONTROL A/B Test]](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=zh-Hant){target=_blank}和[[!UICONTROL Experience Targeting]](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=zh-Hant){target=_blank} (X)T活動現在可以預先擷取SPA[&#128279;](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)的檢視上有修改。

```shell  {line-numbers="true"}
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

上述範例呼叫會預先擷取透過SPA VEC為[!UICONTROL A/B Test]建立的所有檢視，以及要針對網頁`channel`顯示的XT活動。 請注意，呼叫會預先擷取有`tntId`：`84e8d0e211054f18af365d65f45e902b.28_131`的訪客造訪`url`：`https://target.enablementadobe.com/react/demo/#/`時符合資格的[!UICONTROL A/B Test]或XT活動的所有檢視。

```JSON  {line-numbers="true"}
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

在回應的`content`欄位中，備註中繼資料，例如`type`、`selector`、`cssSelector`和`content`，這些中繼資料用於在使用者造訪您的頁面時，將體驗呈現給訪客。 請注意，必要時可以快取`prefetched`內容並轉譯給使用者。
