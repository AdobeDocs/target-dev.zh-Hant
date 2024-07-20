---
title: 使用create方法初始化Java SDK
description: 瞭解如何使用create方法初始化Java SDK並將[!UICONTROL TargetClient]例項化，以呼叫 [!DNL Adobe Target] 進行實驗與個人化體驗。
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
source-git-commit: 1d080b5e402e5d55039bf06611b44678cc6c36de
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 17%

---

# 初始化Java SDK

## 說明

使用`create`方法，以初始化Java SDK並具現化[!UICONTROL Target Client]以呼叫[!DNL Adobe Target]進行實驗與個人化體驗。

## 方法

[!UICONTROL TargetClient]已使用`TargetClient.create`建立。

### 建立

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

已使用`ClientConfig.builder`建立ClientConfig。

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## 參數

`ClientConfigBuilder`具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 使用者端 | 字串 | 是 | 無 | [!UICONTROL Target Client Id] |
| organizationId | 字串 | 是 | 無 | [!UICONTROL Experience Cloud Organization ID] |
| connectTimeout | 數字 | 無 | 10000 | 所有要求的連線逾時（以毫秒為單位） |
| socketTimeout | 數字 | 無 | 10000 | 所有請求的通訊端逾時（以毫秒為單位） |
| maxConnectionsPerHost | 數字 | 無 | 100 | 每個[!DNL Target]主機的最大連線數 |
| maxConnectionsTotal | 數字 | 無 | 200 | 包含所有[!DNL Target]主機的最大連線數 |
| connectionTtlMs | 數字 | 無 | -1 | 總存留時間(TTL)定義持續連線的最大存留時間（以毫秒為單位）。 依預設，連線將無限期地保持連線 |
| idleConnectionValidationMs | 數字 | 無 | 1000 | 持續連線在重複使用之前重新驗證的非使用時間（毫秒） |
| evictIdleConnectionsAfterSecs | 數字 | 無 | 20 | 從連線集區收回閒置連線的時間（秒） |
| enableRetries | 布林值 | 無 | true | 通訊端逾時的自動重試（最多4次） |
| logRequests | 布林值 | 無 | false | 在偵錯中記錄[!DNL Target]個要求與回應 |
| Logrequestatus | 布林值 | 無 | false | 記錄[!DNL Target]回應時間、狀態和URL |
| serverDomain | 字串 | 無 | `*client*.tt.omtrdc.net` | 覆寫預設主機名稱 |
| secure | 布林值 | 無 | true | 取消設定以強制執行HTTP配置 |
| requestInterceptor | HttpRequestInterceptor | 否 | 空 | 新增自訂請求攔截器 |
| defaultPropertyToken | 字串 | 無 | 無 | 設定每個`getOffers`呼叫的預設屬性權杖。 **針對裝置上決策**，SDK只會下載包含在`defaultPropertyToken`中設定的屬性權杖的合格活動的成品 |
| defaultDecisioningMethod | DecisioningMethod列舉 | 否 | SERVER_SIDE | 必須設定為ON_DEVICE或HYBRID才能啟用裝置上決策 |
| telemetryEnabled | 布林值 | 無 | true | 允許客戶在向[!DNL Target]伺服器提出請求時選擇退出其他資料收集 |
| proxyConfig | ClientProxyConfig | 否 | 無 | 允許使用者端提供自己的Proxy詳細資料 |
| exceptionHandler | TargetExceptionHandler | 否 | 無 | 可用於在規則處理期間實作自訂例外狀況處理 |
| httpClient | HttpClient | 否 | 無 | 允許使用者以自訂HTTP使用者端取代[!DNL Target] HTTP使用者端 |
| onDeviceEnvironment | 字串 | 無 | 生產 | 可用來指定不同的裝置上環境，例如測試 |
| Deviceconfighostname | 字串 | 無 | `assets.adobetarget.com` | 可用來指定其他主機來下載裝置上決策成品檔案 |
| onDeviceDecisioningPollingIntSecs | int | 否 | 300 （5分鐘） | 擷取裝置上決策成品檔案的間隔秒數 |
| onDeviceArtifactPayload | 位元組[] | 否 | 無 | 提供裝置上決策，搭配先前的成品裝載，以便立即執行 |
| Devicedecisioninghandler | OnDeviceDecisioningHandler | 否 | 無 | 註冊裝置上決策事件的回呼 |
| onDeviceAllMatchingRulesMboxes | 清單\&lt;字串\> | 否 | 無 | 允許使用者指定在裝置上決策期間將傳回所有相符規則內容的mbox |

## 範例

### Java

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient.create(clientConfig);

// make calls to Adobe Target
```
