---
title: 識別訪客的Adobe Target傳送API
description: 如何在中識別使用者 [!DNL Adobe Target]？
keywords: 傳送api
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---

# 識別訪客

有多種方式可以在中識別訪客 [!DNL Adobe Target].

Target使用三個識別碼：

| 欄位名稱 | 說明 |
| --- | --- |
| `tntId` | 此 `tntId` 是中的主要識別碼 [!DNL Target] 適用於使用者。 您可以提供此ID或 [!DNL Target] 如果請求未包含，則會自動產生。 |
| `thirdPartyId` | 此 `thirdPartyId` 是貴公司可透過每次呼叫傳送之使用者的識別碼。 使用者登入公司網站時，公司通常會建立ID，此ID會連結至訪客的帳戶、熟客卡、會員編號或適用於該公司的其他識別碼。 |
| `marketingCloudVisitorId` | 此 `marketingCloudVisitorId` 用於在不同Adobe解決方案之間合併和共用資料。 此 `marketingCloudVisitorId` 需要，才能與Adobe Analytics和Adobe Audience Manager整合。 |
| `customerIds` | 除了Experience Cloud訪客ID之外， [客戶ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) 而且可以利用每個訪客的已驗證狀態。 |

## [!DNL Target] ID

此 [!DNL Target] ID或 `tntId` 可視為裝置ID。 這個 `tntId` 自動產生者： [!DNL Target] 若請求中未提供。 此後，後續的請求需要包含此 `tntId` 以便向使用者使用的裝置傳遞正確的內容。

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

上述呼叫範例說明 `tntId` 不需要傳入。 在此案例中， [!DNL Target]  產生 `tntId` 並在回應中提供，如下所示：

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

產生的 `tntId` 是 `10abf6304b2714215b1fd39a870f01afc.28_20`. 請注意此 `tntId` 需要在呼叫 [!UICONTROL Adobe Target傳送API] 適用於不同工作階段的相同使用者。

## Marketing Cloud 訪客 ID

此 `marketingCloudVisitorId` 是一個通用且持續儲存的ID，可識別Experience Cloud中所有解決方案的訪客。 當您的組織實作ID服務時，此ID可讓您在不同的Experience Cloud解決方案(如Adobe Target、Adobe Analytics或Adobe Audience Manager)中識別相同的網站訪客及其資料。 請注意 `marketingCloudVisitorId` 運用並與Analytics和Audience Manager整合時需要。

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

上述呼叫範例示範如何 `marketingCloudVisitorId` 從Experience CloudID服務擷取的資料會傳遞至Adobe Target。 在此案例中， [!DNL Target] 產生 `tntId` 因為未傳遞至原始呼叫，而原始呼叫將會對應至提供的 `marketingCloudVisitorId` 如下列回應所示。

## 第三方ID

如果您的組織使用ID來識別訪客，您可以使用 `thirdPartyID` 以傳遞內容。 不過，您必須提供 `thirdPartyID` 代表每 [!UICONTROL Adobe Target傳送API] 撥打您的電話。

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

上述呼叫範例顯示 `thirdPartyId`，這是您的企業用來識別一般使用者的永久ID，無論他們是否透過Web、行動或IoT頻道與您的企業互動。 換言之， `thirdPartyId` 會參照可在各個管道中使用的使用者設定檔資料。 在此案例中， [!DNL Target] 產生 `tntId`，因為它未傳入原始呼叫，而原始呼叫將會對應至提供的 `thirdPartyId` 如下列回應所示。

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

[客戶ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) 可新增並與Experience Cloud訪客ID建立關聯。 每當傳送時 `customerIds` 此 `marketingCloudVisitorId` 也必須提供。 此外，還可提供驗證狀態，以及每個 `customerId` 給每個訪客。 可考慮的驗證狀態如下：

| 驗證狀態 | 使用者狀態 |
| --- | --- |
| `unknown` | 未知或從未驗證此狀態適用於訪客按一下顯示廣告而登陸您的網站等情況。 |
| `authenticated` | 使用者目前是以您網站或應用程式上使用中的工作階段驗證。 |
| `logged_out` | 使用者通過驗證，但主動登出。使用者有意且確定中斷驗證狀態的連結。使用者不希望具有驗證身分。 |

請注意，只有當客戶ID位於 `authenticated` state將會引用Target所儲存並連結至客戶ID的使用者設定檔資料。 如果客戶ID位於 `unknown` 或 `logged_out` 州名、客戶id等，系統將忽略該客戶id，且任何可能與其相關聯的使用者設定檔資料都不會用於對象鎖定目標。

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

上述呼叫範例示範如何傳送 `customerId` 具有 `authenticatedState`. 傳送時 `customerId`，則 `integrationCode`， `id`、和 `authenticatedState` 以及 `marketingCloudVisitorId` 為必要項。 此 `integrationCode` 是的別名 [客戶屬性檔案](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html??lang=zh-Hant) 您透過CRS提供。

## 合併的設定檔

您可以合併 `tntId`， `thirdPartyID`、和 `marketingCloudVisitorId` 在相同要求中。 在此案例中，Adobe Target將維護這些ID的對應，並將其釘選至訪客。 瞭解設定檔如何 [已即時合併和同步](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) 使用不同的識別碼。

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

上述呼叫範例示範如何結合 `tntId`， `thirdPartyID`、和 `marketingCloudVisitorId` 在相同要求中。 回應中也會傳回所有三個ID。
