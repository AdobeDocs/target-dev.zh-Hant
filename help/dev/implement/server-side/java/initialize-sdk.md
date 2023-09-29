---
title: 使用create方法初始化Java SDK
description: 瞭解如何使用create方法初始化Java SDK並例項化 [!UICONTROL TargetClient] 以呼叫 [!DNL Adobe Target] 用於實驗與個人化體驗。
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
source-git-commit: 1d080b5e402e5d55039bf06611b44678cc6c36de
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 17%

---

# 初始化 Java SDK

## 說明

使用 `create` 方法，以初始化Java SDK並將 [!UICONTROL Target使用者端] 以呼叫 [!DNL Adobe Target] 用於實驗與個人化體驗。

## 方法

[!UICONTROL TargetClient] 建立工具： `TargetClient.create`.

### 建立

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

ClientConfig的建立方式 `ClientConfig.builder`.

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## 參數

`ClientConfigBuilder` 具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 使用者端 | 字串 | 是 | 無 | [!UICONTROL Target使用者端Id] |
| organizationId | 字串 | 是 | 無 | [!UICONTROL Experience Cloud組織ID] |
| connectTimeout | 數字 | 無 | 10000 | 所有要求的連線逾時（以毫秒為單位） |
| socketTimeout | 數字 | 無 | 10000 | 所有請求的通訊端逾時（以毫秒為單位） |
| maxConnectionsPerHost | 數字 | 無 | 100 | 每個專案的最大連線數 [!DNL Target] 主機 |
| maxConnectionsTotal | 數字 | 無 | 200 | 最大連線數（包含全部） [!DNL Target] 主機 |
| connectionTtlMs | 數字 | 無 | -1 | 總存留時間(TTL)定義持續連線的最大存留時間（以毫秒為單位）。 依預設，連線將無限期地保持連線 |
| idleConnectionValidationMs | 數字 | 無 | 1000 | 持續連線在重複使用之前重新驗證的非使用時間（毫秒） |
| evictIdleConnectionsAfterSecs | 數字 | 無 | 20 | 從連線集區收回閒置連線的時間（秒） |
| enableRetries | 布林值 | 無 | true | 通訊端逾時的自動重試（最多4次） |
| logRequests | 布林值 | 無 | false | 記錄 [!DNL Target] 偵錯中的要求與回應 |
| Logrequestatus | 布林值 | 無 | false | 記錄 [!DNL Target] 回應時間、狀態和URL |
| serverDomain | 字串 | 無 | `*client*.tt.omtrdc.net` | 覆寫預設主機名稱 |
| secure | 布林值 | 無 | true | 取消設定以強制執行HTTP配置 |
| requestInterceptor | HttpRequestInterceptor | 否 | 空 | 新增自訂請求攔截器 |
| defaultPropertyToken | 字串 | 無 | 無 | 設定預設屬性權杖的間隔 `getOffers` 呼叫。 **針對裝置上決策**，SDK只會下載包含中設定的屬性代號之合格活動的成品 `defaultPropertyToken` |
| defaultDecisioningMethod | DecisioningMethod列舉 | 否 | SERVER_SIDE | 必須設定為ON_DEVICE或HYBRID才能啟用裝置上決策 |
| telemetryEnabled | 布林值 | 無 | true | 允許客戶在收到以下請求時選擇退出其他資料收集： [!DNL Target] 伺服器 |
| proxyConfig | ClientProxyConfig | 否 | 無 | 允許使用者端提供自己的Proxy詳細資料 |
| exceptionHandler | TargetExceptionHandler | 否 | 無 | 可用於在規則處理期間實作自訂例外狀況處理 |
| httpClient | HttpClient | 否 | 無 | 允許使用者取代 [!DNL Target] 具有自訂HTTP使用者端的HTTP使用者端 |
| onDeviceEnvironment | 字串 | 無 | 生產 | 可用來指定不同的裝置上環境，例如測試 |
| Deviceconfighostname | 字串 | 無 | `assets.adobetarget.com` | 可用來指定其他主機來下載裝置上決策成品檔案 |
| onDeviceDecisioningPollingIntSecs | int | 否 | 300 （5分鐘） | 擷取裝置上決策成品檔案的間隔秒數 |
| onDeviceArtifactPayload | 位元組[] | 否 | 無 | 提供裝置上決策，搭配先前的成品裝載，以便立即執行 |
| Devicedecisioninghandler | OnDeviceDecisioningHandler | 否 | 無 | 註冊裝置上決策事件的回呼 |
| onDeviceAllMatchingRulesMboxes | 清單\&lt;string> | 否 | 無 | 允許使用者指定在裝置上決策期間將傳回所有相符規則內容的mbox |

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
