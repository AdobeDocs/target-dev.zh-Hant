---
title: 使用create方法初始化.NET SDK
description: 瞭解如何使用create方法初始化Java SDK並將[!UICONTROL TargetClient]例項化，以呼叫 [!DNL Adobe Target] 進行實驗與個人化體驗。
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 16%

---

# 初始化.NET SDK

## 說明

使用`Create`方法，以初始化.NET SDK並將[!UICONTROL Target Client]例項化，以呼叫[!DNL Adobe Target]進行實驗與個人化體驗。

使用.NET相依性插入時，只要在服務設定步驟中呼叫`services.AddTargetLibrary()`來新增SDK，然後將`ITargetClient targetClient`插入應用程式的建構函式。

之後，使用SDK的`Initialize`方法設定SDK，完成初始化步驟。

## 方法

`TargetClient`已使用`TargetClient.Create`建立。

## C\
#

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig`是使用ClientConfig.Builder建立的。

## C\
#

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## 參數

`TargetClientConfig.Builder`具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 用戶端 | string | 是 | 無 | [!UICONTROL Target Client Id] |
| OrganizationId | string | 是 | 無 | [!UICONTROL Experience Cloud Organization ID] |
| 逾時 | int | 否 | 10000 | 所有要求的逾時（以毫秒為單位） |
| Proxy |  | WebProxy | 否 | null | 所有[!DNL Target]要求的Proxy |
| 重試原則 | 原則 | 否 | null | 重試所有[!DNL Target]要求的原則 |
| AsyncRetryPolicy | AsyncPolicy | 否 | null | 所有[!DNL Target]要求的非同步重試原則 |
| Logger | ILogger | 否 | null | 用於[!DNL Target]個要求與回應的偵錯記錄 |
| ServerDomain | string | 否 | `client.tt.omtrdc.net` | 覆寫預設主機名稱 |
| 安全 | 布林值 | 否 | true | 取消設定以強制執行HTTP配置 |
| DefaultpropertyToken | string | 否 | null | 設定每個`getOffers`呼叫的預設屬性權杖 |
| TelemetryEnabled | 布林值 | 否 | true | 傳送遙測資料以改善SDK使用體驗 |
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
