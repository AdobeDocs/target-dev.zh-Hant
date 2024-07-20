---
title: Adobe Target傳送API單一或批次傳送
description: 如何使用[!UICONTROL Adobe Target Delivery API]單一或批次傳遞呼叫？
keywords: 傳送api
exl-id: 525cd1f2-616a-486c-8f49-8117615500bb
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# 單一或批次傳遞

[!UICONTROL Adobe Target Delivery API]支援單一或批次傳遞呼叫。 您可以對單一或多個mbox的內容提出伺服器請求。

在決定進行單一呼叫與批次呼叫時，權衡效能成本。 如果您知道需要為使用者顯示的所有內容，最佳實務是使用單一批次傳遞呼叫為所有mbox擷取內容，以避免進行多個單一傳遞呼叫。

## 單一傳遞呼叫

您可以透過[!UICONTROL Adobe Target Delivery API]擷取要向使用者顯示一個mbox的體驗。 請注意，如果您進行單一傳遞呼叫，則需要起始另一個伺服器呼叫，以便為使用者擷取mbox的其他內容。 隨著時間推移，這可能會變得非常昂貴，因此，請務必在使用單一「傳送API」呼叫時評估您的方法。

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
    "execute": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      }
    ]
  }
}'
```

在上述單一傳遞呼叫範例中，已擷取體驗，以顯示給網頁頻道上具有`mbox`：`SummerOffer`之`tntId`： `abcdefghijkl00023.1_1`的使用者。 此單一傳遞呼叫將產生下列回應：

```
{
  "status": 200,
  "requestId": "25e0cc42-3d7b-456a-8b49-af60c1fb23d9",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.1_1"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",
                  }
              ]
          }
      ]
    }
}
```

在回應中，請注意`content`欄位包含的HTML說明將向使用者顯示的對應於SummerOffer mbox之網頁體驗。

### 執行頁面載入

如果在Web Channel中發生頁面載入時，有應顯示的體驗（例如AB測試頁尾或頁首中的字型），您可以在`execute`欄位中指定`pageLoad`以擷取應套用的所有修改。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359570e04f14e1faeeba02d6ab9914e' \
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
  "execute": {
    "pageLoad": {}
  }
}'
```

上述範例呼叫會擷取任何體驗，以在頁面`https://target.enablementadobe.com/react/demo/#/`載入時顯示使用者。

```
{
      "status": 200,
      "requestId": "355ebc47-edb6-481f-aeae-ae55d71afaca",
      "client": "demo",
      "id": {
          "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
      },
      "edgeHost": "mboxedge28.tt.omtrdc.net",
      "execute": {
          "pageLoad": {
              "options": [
                  {
                      "content": [
                          {
                              "type": "setHtml",
                              "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV.nav:eq(0) > DIV.container:eq(0) > DIV.nav-right:eq(0) > A.nav-item:eq(0)",
                              "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > A:nth-of-type(1)",
                              "content": "Modified Home"
                          }
                      ],
                      "type": "actions"
                  }
              ],
              "metrics": [
                  {
                      "type": "click",
                      "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > FORM.col-md-4:eq(0) > DIV.form-group:eq(0) > BUTTON.btn:eq(0)",
                      "eventToken": "QPaLjCeI9qKCBUylkRQKBg=="
                  }
              ]
          }
      }
  }
```

在`content`欄位中，可以擷取需要套用至頁面載入的修改。 在上述範例中，請注意，標頭上的連結必須命名為&#x200B;*Modified Home*。

## 批次傳遞呼叫

與其透過每個呼叫中的單一mbox進行多個傳遞呼叫，使用批次mbox進行一個傳遞呼叫可以減少不必要的伺服器呼叫。 叫用伺服器呼叫時應該儘可能最小化，以便擁有高效能。

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
    "execute": {
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

在上述批次傳遞呼叫範例中，已擷取體驗，以針對多個`mbox`：`SummerOffer`、`SummerShoesOffer`和`SummerDressOffer`顯示`tntId`： `abcdefghijkl00023.1_1`的使用者。 由於我們瞭解需要為此使用者顯示多個mbox的體驗，因此我們可以批次處理這些請求，並進行一個伺服器呼叫，而不是三個個別的傳送呼叫。

```
{
  "status": 200,
  "requestId": "fe15286f-effb-434f-85d8-c3db804075ce",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_120"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",

                  }
              ]
          },
          {
              "index": 2,
              "name": "SummerShoesOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next shoe purchase</b></p>",
                      "type": "html",
                  }
              ]
          },
          {
              "index": 3,
              "name": "SummerDressOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next dress purchase</b></p>",
                      "type": "html",
                  }
              ]
          }
      ]
  }
}
```

在上述回應中，您可以看到在每個mbox的`content`欄位中，可以擷取每個mbox要向使用者顯示的體驗HTML表示法。
