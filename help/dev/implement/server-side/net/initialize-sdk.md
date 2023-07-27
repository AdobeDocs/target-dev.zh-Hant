---
title: 使用create方法初始化.NET SDK
description: 瞭解如何使用create方法初始化Java SDK並例項化 [!UICONTROL TargetClient] 以呼叫 [!DNL Adobe Target] 用於實驗與個人化體驗。
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 17%

---

# 初始化.NET SDK

## 說明

使用 `Create` 方法，以初始化.NET SDK並將 [!UICONTROL Target使用者端] 以呼叫 [!DNL Adobe Target] 用於實驗與個人化體驗。

使用.NET相依性插入時，只需在服務設定步驟中新增SDK，做法是呼叫 `services.AddTargetLibrary()`；然後插入 `ITargetClient targetClient` 在您應用程式的建構函式中。

之後，使用 `Initialize` SDK的方法來設定SDK，進而完成初始化步驟。

## 方法

`TargetClient` 建立工具： `TargetClient.Create`.

## C\
#

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig` 使用ClientConfig.Builder建立。

## C\
#

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## 參數

`TargetClientConfig.Builder` 具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 客戶 | string | 是 | 無 | [!UICONTROL Target使用者端Id] |
| OrganizationId | string | 是 | 無 | [!UICONTROL Experience Cloud組織ID] |
| 逾時 | int | 否 | 10000 | 所有要求的逾時（以毫秒為單位） |
| Proxy |  | WebProxy | 否 | null | 全部的Proxy [!DNL Target] 請求 |
| 重試原則 | 原則 | 否 | null | 全部重試原則 [!DNL Target] 請求 |
| AsyncRetryPolicy | AsyncPolicy | 否 | null | 所有使用者非同步重試原則 [!DNL Target] 請求 |
| Logger | ILogger | 否 | null | 用於的偵錯記錄 [!DNL Target] 要求和回應 |
| ServerDomain | string | 否 | `client.tt.omtrdc.net` | 覆寫預設主機名稱 |
| 安全 | bool | 否 | true | 取消設定以強制執行HTTP配置 |
| DefaultpropertyToken | string | 否 | null | 設定預設屬性權杖的間隔 `getOffers` 呼叫 |
| TelemetryEnabled | bool | 否 | true | 傳送遙測資料以改善SDK使用體驗 |
| 決策方法 | DecisioningMethod列舉 | 否 | ServerSide | 必須設定為OnDevice或Hybrid才能啟用裝置上決策 |
| OndevicedecisioningReady | Action | 否 | null | 委派裝置上決策就緒事件（當裝置上決策就緒時呼叫一次） |
| ArtifactDownloadSucceeded | Action | 否 | null | 委派裝置上決策成品下載成功（在每次成功下載成品時呼叫） |
| ArtifactDownloadFailed | Action | 否 | null | 裝置上決策成品下載失敗的委派（在每次失敗的成品下載時呼叫） |
| OnDeviceEnvironment | string | 否 | 生產 | 可用來指定不同的裝置上環境，例如中繼環境 |
| OnDeviceConfigHostname | string | 否 | `assets.adobetarget.com` | 可用來指定其他主機來下載裝置上決策成品檔案 |
| OnDeviceDecisioningPollingIntSecs | int | 否 | 300 （5分鐘） | 擷取裝置上決策成品檔案的間隔秒數 |
| OnDeviceArtifactPayload | string | 否 | null | 提供具有本機成品裝載的裝置上決策，以允許立即執行 |

## 範例

## C\
#

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeclient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

targetClient = TargetClient.Create(targetClientConfig);

// make calls to Adobe Target
```
