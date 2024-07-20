---
title: 識別訪客的Adobe Target傳送API
description: 如何識別 [!DNL Adobe Target]中的使用者？
keywords: 傳送api
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 7%

---

# 識別訪客

在[!DNL Adobe Target]內有多種方式可用來識別訪客。

Target使用三個識別碼：

| 欄位名稱 | 說明 |
| --- | --- |
| `tntId` | `tntId`是使用者[!DNL Target]中的主要識別碼。 您可以提供此ID，或如果請求未包含此ID，則[!DNL Target]將自動產生此ID。 |
| `thirdPartyId` | `thirdPartyId`是您公司可透過每次呼叫傳送之使用者的識別碼。 使用者登入公司網站時，公司通常會建立ID，此ID會連結至訪客的帳戶、熟客卡、會員編號或適用於該公司的其他識別碼。 |
| `marketingCloudVisitorId` | `marketingCloudVisitorId`用於合併和共用不同Adobe解決方案之間的資料。 必須有`marketingCloudVisitorId`才能與Adobe Analytics和Adobe Audience Manager整合。 |
| `customerIds` | 除了Experience Cloud訪客ID之外，還可以使用其他[客戶ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html)以及每個訪客的已驗證狀態。 |

## [!DNL Target] ID

[!DNL Target] ID或`tntId`可視為裝置ID。 如果要求中未提供此`tntId`，[!DNL Target]會自動產生它。 此後，後續請求需要包含此`tntId`，才能將正確的內容傳遞至使用者使用的裝置。

```http {line-numbers="true"}
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
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
        "name" : "SummerOffer",
        "index" : 1
      }
    ]
  }
}'
```

上述呼叫範例說明`tntId`不需要傳入。 在此案例中，[!DNL Target]會產生`tntId`並在回應中提供它，如下所示：

```URI {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "demo",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  ...
}
```

產生的`tntId`為`10abf6304b2714215b1fd39a870f01afc.28_20`。 請注意，跨工作階段呼叫相同使用者的[!UICONTROL Adobe Target Delivery API]時，需要使用此`tntId`。

## Marketing Cloud 訪客 ID

`marketingCloudVisitorId`是通用的永久ID，可識別Experience Cloud中所有解決方案的訪客。 當您的組織實作ID服務時，此ID可讓您在不同的Experience Cloud解決方案(如Adobe Target、Adobe Analytics或Adobe Audience Manager)中識別相同的網站訪客及其資料。 請注意，當運用並與Analytics和Audience Manager整合時，需要`marketingCloudVisitorId`。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "marketingCloudVisitorId": "10527837386392355901041112038610706884"
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

上述範例呼叫示範如何將從Experience CloudID服務擷取的`marketingCloudVisitorId`傳遞至Adobe Target。 在此案例中，[!DNL Target]會產生`tntId`，因為它未傳入原始呼叫，而原始呼叫將會對應到提供的`marketingCloudVisitorId`，如下面的回應所示。

## 第三方ID

如果您的組織使用ID來識別訪客，您可以使用`thirdPartyID`來傳遞內容。 不過，您必須為每[!UICONTROL Adobe Target Delivery API]次呼叫提供`thirdPartyID`。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "thirdPartyId": "B234A029348"
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

上述呼叫範例顯示`thirdPartyId`，這是您企業用來識別一般使用者的永久ID，無論他們是否透過Web、行動裝置或IoT管道與您的企業互動。 換言之，`thirdPartyId`將參照可在不同管道間使用的使用者設定檔資料。 在此案例中，[!DNL Target]會產生`tntId`，因為它未傳入原始呼叫，而將會對應到提供的`thirdPartyId`，如下面的回應所示。

```
{
    "status": 200,
    "requestId": "55de9886-bd14-4dee-819c-7d1633b79b90",
    "client": "demo",
    "id": {
        "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20",
        "thirdPartyId": "B234A029348"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    ...
}
```

## Customer ID

可以新增[客戶ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html)，並將其與Experience Cloud的訪客ID建立關聯。 每當您傳送`customerIds`時，也必須提供`marketingCloudVisitorId`。 此外，可以為每個訪客提供驗證狀態，以及每個`customerId`。 可考慮的驗證狀態如下：

| 驗證狀態 | 使用者狀態 |
| --- | --- |
| `unknown` | 未知或從未驗證。 此狀態適用於訪客按一下顯示廣告而登陸您的網站等情況。 |
| `authenticated` | 使用者目前是以您網站或應用程式上使用中的工作階段驗證。 |
| `logged_out` | 使用者通過驗證，但主動登出。使用者有意且確定中斷驗證狀態的連結。使用者不希望具有驗證身分。 |

請注意，只有當客戶ID處於`authenticated`狀態時，Target才會參考已儲存並連結至客戶ID的使用者設定檔資料。 如果客戶ID處於`unknown`或`logged_out`狀態，將忽略客戶ID，並且不會將任何可能與其相關聯的使用者設定檔資料用於對象定位。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "customerIds": [{
          "id": "134325423",
          "integrationCode" : "crm_data",
          "authenticatedState" : "authenticated"
        }]
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

上述呼叫範例示範如何傳送含有`authenticatedState`的`customerId`。 傳送`customerId`時，需要`integrationCode`、`id`、`authenticatedState`以及`marketingCloudVisitorId`。 `integrationCode`是您透過CRS提供的[客戶屬性檔案](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html??lang=zh-Hant)的別名。

## 合併的設定檔

您可以在同一個要求中合併`tntId`、`thirdPartyID`和`marketingCloudVisitorId`。 在此案例中，Adobe Target將維護這些ID的對應，並將其釘選至訪客。 瞭解如何使用不同的識別碼[即時合併及同步設定檔](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html)。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "tntId": "d359234570e044f14e1faeeba02d6ab23439914e.28_78",
        "thirdPartyId":"23423432"
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

上述呼叫範例示範如何在相同要求中合併`tntId`、`thirdPartyID`和`marketingCloudVisitorId`。 回應中也會傳回所有三個ID。
