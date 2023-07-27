---
title: 與Experience Cloud A4T報表整合
description: 與Experience Cloud、A4T報表、Analytics for Target整合的整合
keywords: 傳送api，伺服器端，伺服器端，整合， a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 6%

---

# Analytics for Target (A4T) 報告

[!DNL Adobe Target] 裝置上決策和伺服器端Target活動皆支援A4T報告。 啟用A4T報表時有兩個設定選項：

* [!DNL Adobe Target] 自動將分析裝載轉送至 [!DNL Adobe Analytics]，或
* 使用者向請求分析裝載 [!DNL Adobe Target]. ([!DNL Adobe Target] 傳回 [!DNL Adobe Analytics] 裝載傳回給呼叫者。)

>[!NOTE]
>
>裝置上決策僅支援的A4T報告 [!DNL Adobe Target] 自動將分析裝載轉送至 [!DNL Adobe Analytics]. 正在擷取分析裝載，從 [!DNL Adobe Target] 不受支援。

## 先決條件

1. 在中設定活動 [!DNL Adobe Target] UI與 [!DNL Adobe Analytics] 作為報表來源，並確認帳戶已啟用A4T。
1. API使用者會產生Adobe Marketing Cloud訪客ID，並確保此ID在執行Target請求時可供使用。

## [!DNL Adobe Target] 自動轉送Analytics裝載

[!DNL Adobe Target] 可自動將分析裝載轉送至 [!DNL Adobe Analytics] 如果提供了以下識別碼：

1. `supplementalDataId`：用於拼接的ID [!DNL Adobe Analytics] 和 [!DNL Adobe Target]. 為了 [!DNL Adobe Target] 和 [!DNL Adobe Analytics] 若要正確地將資料彙整在一起，請 `supplementalDataId` 需要傳遞至兩者 [!DNL Adobe Target] 和 [!DNL Adobe Analytics].
1. `trackingServer`：此 [!DNL Adobe Analytics] 伺服器。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "server_side",
        supplementalDataId: "7D3AA246CC99FD7F-1B3DD2E75595498E",
        trackingServer: "jimsbrims.sc.omtrds.net"
      }
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

>[!TAB Java]

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

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .trackingServer("jimsbrims.sc.omtrds.net")
        .logging(LoggingType.SERVER_SIDE)
        .supplementalDataId("7D3AA246CC99FD7F-1B3DD2E75595498E");
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

## 使用者從以下位置擷取分析裝載： [!DNL Adobe Target]

使用者可以擷取 [!DNL Adobe Analytics] 給定mbox的裝載，然後傳送至 [!DNL Adobe Analytics] 透過 [資料插入API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). 當 [!DNL Adobe Target] 請求已引發，通過 `client_side` 至 `logging` 要求中的欄位。 如果在使用Analytics作為報表來源的活動中存在指定的mbox，則會傳回裝載。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};
const targetClient = TargetClient.create(CONFIG);
targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "client_side"
      }
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

>[!TAB Java]

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

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .logging(LoggingType.CLIENT_SIDE);
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

一旦您指定 `logging = client_side`，您將會在mbox欄位中接收裝載。

如果來自Target的回應包含 `analytics -> payload` 屬性，直接轉寄至 [!DNL Adobe Analytics]. [!DNL Adobe Analytics] 知道如何處理此裝載。 您可使用下列格式，在GET要求中完成此作業：

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### 查詢字串引數和變數

| 欄位名稱 | 必要 | 說明 |
| --- | --- | --- |
| `rsid` | 是 | 報表套裝 ID |
| `pe` | 是 | 頁面事件。 一律設為 `tnt` |
| `tnta` | 是 | Target伺服器傳回的Analytics裝載，位於 `analytics -> payload -> tnta` |
| `mid` | 是 | Marketing Cloud 訪客 ID |

### 必要的標頭值

| 頁首名稱 | 標頭值 |
| --- | --- |
| 主機 | Analytics資料收集伺服器(如： `adobeags421.sc.omtrdc.net`) |

### 範例A4T資料插入HTTP Get呼叫

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```
