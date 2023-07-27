---
title: 與Experience Cloud整合
description: 與Experience Cloud整合
keywords: 傳送api
source-git-commit: f16903556954d2b1854acd429f60fbf6fc2920de
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 7%

---

<!-- The content on this page was originally pulled from the legacy Delivery API doc site, and was subsequently found to be an outdated version of information now found on [Implement > Server-side > Integration > A4T Reporting](../../implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md) and [Implement > Server-side > Integration > AAM Segments](../../implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md). Therefore sunsetting this page, but not deleting it from the repo immediately, pending a more thorough examination to avoid inadvertently deleting relevant content. -->


# 與Experience Cloud整合

## Adobe 目標分析 (A4T)

從伺服器觸發Target傳送API呼叫時，Adobe Target會傳回該使用者的體驗，此外，Adobe Target會將Adobe Analytics裝載傳回給呼叫者，或自動將其轉送至Adobe Analytics。 若要在伺服器端將Target活動資訊傳送至Adobe Analytics，請先滿足幾個必要條件：

1. 活動是在Adobe Target UI中設定，以Adobe Analytics作為報表來源，且帳戶已啟用A4T
1. Adobe Marketing Cloud訪客ID是由API使用者產生，並在觸發Target傳送API呼叫時可供使用

### Adobe Target自動轉送分析裝載

如果提供了以下識別碼，Adobe Target可以自動透過伺服器端轉送analytics裝載至Adobe Analytics：

1. `supplementalDataId`  — 用來在Adobe Analytics和Adobe Target之間拼接的ID
1. `trackingServer` -AdobeAnalytics伺服器為了讓Adobe Target和Adobe Analytics能夠正確地彙整資料，同樣地 `supplementalDataId` 需要傳遞至Adobe Target和Adobe Analytics。

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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "analytics": {
          "supplementalDataId" : "23423498732598234",
          "trackingServer": "ags041.sc.omtrdc.net",
          "logging": "server_side"
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

### 從Adobe Target擷取Analytics裝載

Adobe Target Delivery API的消費者可以擷取相對應mbox的Adobe AnalyticsAdobe Analytics裝載，因此消費者可以透過 [資料插入API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). 觸發伺服器端Adobe Target呼叫時，傳遞 `client_side` 至 `logging` 要求中的欄位。 如果mbox存在於使用Analytics作為報表來源的活動中，則會傳回裝載。

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
      "experienceCloud": {
        "analytics": {
          "logging": "client_side"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
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

一旦您指定 `logging` = `client_side`，您會在以下位置接收裝載： `mbox` 欄位，如下所示。

```
{
    "status": 200,
    "requestId": "4b8855a5-8354-4ac4-8ae7-c551f7c0bb8a",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                        "type": "html",

                    }
                ],
                "analytics": {
                    "payload": {
                        "pe": "tnt",
                        "tnta": "285408:0:0|2"
                    }
                }
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

如果來自Target的回應包含 `analytics` -> `payload` 屬性，直接轉送至Adobe Analytics。 Analytics知道如何處理此裝載。 您可使用下列格式，在GET要求中完成此作業：

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### 查詢字串引數和變數

| 欄位名稱 | 必要 | 說明 |
| --- | --- | --- |
| `rsid` | 是 | 報表套裝 ID |
| `pe` | 是 | 頁面事件。 一律設為 `tnt` |
| `tnta` | 是 | Target伺服器傳回的Analytics裝載，位於 `analytics` -> `payload` -> `tnta` |
| `mid` | Marketing Cloud 訪客 ID |

### 必要的標頭值

| 頁首名稱 | 標頭值 |
| --- | --- |
| 主機 | Analytics資料收集伺服器(如： adobeags421.sc.omtrdc.net) |

### 範例A4T資料插入HTTP Get呼叫

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```

## Adobe Audience Manager

Adobe Audience Manager (AAM)區段也可以透過Adobe Target Delivery API運用。 為了善用AAM區段，必須提供下列欄位：

| 欄位名稱 | 必要 | 說明 |
| --- | --- | --- |
| `locationHint` | 是 | DCS位置提示用於決定要點選哪個AAM DCS端點以擷取設定檔。 必須>= 1。 |
| `marketingCloudVisitorId` | 是 | Marketing Cloud 訪客 ID |
| `blob` | 是 | AAM Blob可用來傳送其他資料給AAM。 不得空白且大小&lt;= 1024。 |

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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "audienceManager": {
          "locationHint": 9,
          "blob": "32fdghkjh34kj5h43"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
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
