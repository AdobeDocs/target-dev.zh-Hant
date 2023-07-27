---
title: 使用者識別與分組
description: 使用者識別與分組
exl-id: 4fcf235b-6a58-442c-ae13-9d05ec1033fc
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 5%

---

# 使用者識別與分組

## 使用者識別

識別使用者的方式有很多種 [!DNL Adobe Target]. [!UICONTROL Target] 會使用以下識別碼：

| 欄位名稱 | 說明 |
| --- | --- |
| `tntID` | 此 `tntId` 是中的主要識別碼 [!DNL Target] 適用於使用者。 您可以提供此ID，或 [!DNL Target] 如果請求未包含，則會自動產生。 |
| `thirdPartyId` | 此 `thirdPartyId` 是您公司的使用者識別碼，可隨每次呼叫傳送。 使用者登入公司網站時，公司通常會建立ID，此ID會連結至訪客的帳戶、熟客卡、會員編號或適用於該公司的其他識別碼。 |
| `marketingCloudVisitorId` | 此 `marketingCloudVisitorId` 用於在不同Adobe解決方案之間合併和共用資料。 與Adobe Analytics和Adobe Audience Manager整合需要marketingCloudVisitorId。 |
| `customerIds` | 除了Experience Cloud訪客ID之外， [客戶ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) 也可以利用每位訪客的已驗證狀態。 |

## [!DNL Target] ID (tntID)

此 [!DNL Target] ID，或 `tntId`，可視為裝置ID。 這個 `tntId` 自動產生者： [!DNL Adobe Target] 若請求中未提供。 後續請求需要包含此專案 `tntId` 以便將正確的內容傳遞至相同使用者所使用的裝置。

下列範例呼叫示範下列情況： `tntId` 未傳遞給 [!DNL Target].

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

如果沒有 `tntId`， [!DNL Adobe Target] 產生 `tntId` 並在回應中提供，如下所示。

```json {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "acmeclient",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.35_0"
  },
  "edgeHost": "mboxedge35.tt.omtrdc.net",
  ...
}
```

在此範例中，已產生的 `tntId` 是 `10abf6304b2714215b1fd39a870f01afc.35_0`. 請注意此 `tntId` 需要跨工作階段用於相同的使用者。

## 第三方ID (thirdPartyId)

如果您的組織使用ID來識別訪客，您可以使用 `thirdPartyID` 以傳遞內容。 A `thirdPartyID` 是永續性ID，您的企業可使用此項ID來識別一般使用者，無論他們是否透過Web、行動或IoT管道與您的企業互動。 換言之， `thirdPartyId` 會參照可在各個管道中使用的使用者設定檔資料。 不過，您必須提供 `thirdPartyID` 代表每 [!DNL Adobe Target] 您進行的傳送API呼叫。

下列範例呼叫示範如何使用 `thirdPartyId`.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      thirdPartyId: "B234A029348"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .thirdPartyId("B234A029348");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

在此案例中， [!DNL Adobe Target] 將產生 `tntId` 因為它未傳遞至原始呼叫，而原始呼叫將會對應至提供的 `thirdPartyId`.

## Marketing Cloud訪客ID (marketingCloudVisitorId)

此 `marketingCloudVisitorId` 是通用的永久性ID，可識別Adobe Experience Cloud所有解決方案的訪客。 當您的組織實作ID服務時，此ID可讓您在不同的Experience Cloud解決方案中識別相同的網站訪客及其資料，包括 [!DNL Adobe Target]、Adobe Analytics和Adobe Audience Manager。 請注意 `marketingCloudVisitorId` 整合時需要 [!DNL Target] 替換為 [!DNL Adobe Analytics] 和 [!DNL Adobe Audience Manager].

下列範例呼叫示範如何 `marketingCloudVisitorId` 從Experience CloudID服務擷取的資料傳遞至 [!DNL Target].

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId: "10527837386392355901041112038610706884"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

在此案例中， [!DNL Target] 將產生 `tntId` 因為它未傳遞至原始呼叫，而原始呼叫將會對應至提供的 `marketingCloudVisitorId`.

## 客戶ID (customerIds)

[客戶ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html) 可新增至Experience Cloud訪客ID或與其相關聯。 每當傳送時 `customerIds`，則 `marketingCloudVisitorId` 也必須提供。 此外，還可提供驗證狀態，以及每個 `customerId` 給每個訪客。 可以使用以下驗證狀態：

| 驗證狀態 | 使用者狀態 |
| --- | --- |
| `unknown` | 未知或從未驗證此狀態適用於訪客透過按一下顯示廣告而登陸您的網站等情境。 |
| `authenticated` | 使用者目前是以您網站或應用程式上使用中的工作階段驗證。 |
| `logged_out` | 使用者通過驗證，但主動登出。使用者打算中斷已驗證狀態的連線。 使用者不希望具有驗證身分。 |

請注意，僅當 `customerId` 處於已驗證狀態將 [!DNL Target] 參考儲存並連結至customerId的使用者設定檔資料。 如果 `customerId` 位於未知或 `logged_out` ，將會被忽略，以及任何可能與其相關聯的使用者設定檔資料 `customerId` 將不會用於對象目標定位。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId : "10527837386392355901041112038610706884",
      customerIds: [{
        id: "134325423",
        integrationCode : "crm_data",
        authenticatedState : "authenticated"
      }]
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

CustomerId customerId = new CustomerId()
  .id("134325423")
  .integrationCode("crm_data")
  .authenticatedState(AuthenticatedState.AUTHENTICATED);
VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884")
  .customerIds(Arrays.asList(customerId));
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

上述範例示範如何傳送 `customerId` 具有 `authenticatedState`. 傳送時 `customerId`，則 `integrationCode`， `id`、和 `authenticatedState` 以及 `marketingCloudVisitorId` 為必要項。 此 `integrationCode` 是的別名 [客戶屬性檔案](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html??lang=zh-Hant) 您透過CRS提供。

## 合併的設定檔

您可以合併 `tntId`， `thirdPartyID`、和 `marketingCloudVisitorId` 在相同要求中。 在此案例中， [!DNL Adobe Target] 將維護這些ID的對應，並將其釘選到訪客。 瞭解設定檔如何 [已即時合併和同步](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) 使用不同的識別碼。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId: "B234A029348",
      marketingCloudVisitorId : "10527837386392355901041112038610706884"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .tntId("d359234570e044f14e1faeeba02d6ab23439914e.35_0")
  .thirdPartyId("B234A029348")
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

上述範例示範如何結合 `tntId`， `thirdPartyID`、和 `marketingCloudVisitorId` 在相同要求中。

## 分段

系統會根據您設定的方式，將您的使用者分段，以便檢視體驗。 [!DNL Adobe Target] 活動。 在 [!DNL Adobe Target]，分組為：

* **確定性**：MurmurHash3是用來確保您的使用者已分組，而且只要使用者ID一致，每次都會看到正確的變數。
* **粘性**： [!DNL Adobe Target] 會儲存您的使用者在使用者設定檔中看到的變數，以確保該使用者在不同工作階段和管道中都能看到這些變數。 使用伺服器端決策功能時，可確保變異和粘著度。 使用裝置上決策時，無法保證粘著度。

## 端對端分組工作流程

在深入研究實際分組演演算法之前，請務必強調，類似的步驟既可用於根據其流量分配百分比來選取活動，也可用於選取活動內的體驗。

### 活動選擇步驟

1. 產生裝置ID，通常是UUID
1. 取得使用者端代碼
1. 取得活動ID
1. 取得salt，這通常是一些字串，例如「activity」
1. 使用MurmurHash3計算雜湊
1. 取得雜湊的絕對值
1. 將雜湊絕對值除以10000
1. 餘數除以10000，這會產生介於0和1之間的值
1. 將結果乘以100%
1. 比較活動流量分配百分比與取得的百分比。 如果流量分配百分比較低，則會選取活動。 否則，會略過活動。

### 體驗選取步驟

1. 產生裝置ID，通常是UUID
1. 取得使用者端代碼
1. 取得活動ID
1. 取得Salt，這通常是「體驗」之類的字串
1. 使用MurmurHash3計算雜湊
1. 取得雜湊的絕對值
1. 將雜湊絕對值除以10000
1. 餘數除以10000，這會產生介於0和1之間的值
1. 將結果乘以活動內的體驗總數
1. 舍入結果。 這應該會產生體驗索引。

### 範例

假設以下事項：

* 具有使用者端代碼的使用者端C `acmeclient`
* 具有ID的活動A `1111` 和三個體驗 `E1`， `E2`， `E3`
* 體驗的分佈如下： `E1` - 33%， `E2` - 33%， `E3` - 34%

選取流程看起來像這樣：

1. 裝置 ID `702ff4d0-83b1-4e2e-a0a6-22cbe460eb15`
1. 用戶端代碼 `acmeclient`
1. 活動 ID `1111`
1. 鹽 `experience`
1. 雜湊的值 `acmeclient.1111.702ff4d0-83b1-4e2e-a0a6-22cbe460eb15.experience`，雜湊值 `-919077116`
1. 雜湊的絕對值 `919077116`
1. 除以10000後的餘數， `7116`
1. 餘數後的值除以10000， `0.7116`
1. 將值與體驗總數相乘後的結果 `3 * 0.7116 = 2.1348`
1. 體驗索引為 `2`，這表示第三次體驗，因為我們使用 `0` 根據索引。
