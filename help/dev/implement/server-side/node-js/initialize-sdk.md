---
title: 使用create方法初始化Node.js SDK
description: 瞭解如何使用create方法初始化Node.js SDK並例項化 [!DNL Target] 要呼叫的使用者端 [!DNL Adobe Target] 用於實驗與個人化體驗。
feature: APIs/SDKs
exl-id: 71516e44-508a-4d8d-9f2b-7c54243e9c60
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 18%

---

# 初始化Node.js SDK

## 說明

使用 `create` 方法，以初始化Node.js SDK並將 [!UICONTROL Target] 要呼叫的使用者端 [!DNL Adobe Target] 用於實驗與個人化體驗。

## 方法

**建立**

```js {line-numbers="true"}
TargetClient.create(options: Object): TargetClient
```

## 參數

`options` 具有以下結構：

| 名稱 | 類型 | 必要 | 預設值 | 說明 |
| --- | --- | --- | --- | --- |
| 使用者端 | 字串 | 是 | 無 | [!UICONTROL Adobe Target使用者端ID] |
| organizationId | 字串 | 是 | 無 | [!UICONTROL Experience Cloud組織ID] |
| 環境 | 字串 | 無 | 生產 | 目標環境名稱。 在 [!DNL Target] UI、 [!UICONTROL 管理] > [!UICONTROL 環境]. |
| timeout | 數字 | 無 | 3000 | 逾時（毫秒） |
| serverDomain | 字串 | 無 | `*client*.tt.omtrdc.net` | 覆寫預設主機名稱 |
| secure | 布林值 | 無 | true | 取消設定以強制執行HTTP配置 |
| logger | 物件 | 無 | NOOP記錄器 | 取代預設的NOOP記錄器 |
| targetLocationHint | 字串 | 無 | 無 | Target位置提示 |
| fetchApi | 函數 | 無 | global.fetch或window.fetch | [擷取](https://fetch.spec.whatwg.org/) SDK會針對http要求使用。 依預設，會使用節點擷取或擷取的瀏覽器實作。 但可使用提供替代實作 `fetchApi` |
| propertyToken | 字串 | 無 | 無 | **目標屬性Token**. 若在此指定，所有 `getOffers` 呼叫將使用此值。 **針對裝置上決策**，SDK只會下載包含中設定的屬性代號之合格活動的成品 `propertyToken` |
| 決策方法 | 字串 | 無 | 伺服器端 | 決定要使用的決策方法([裝置上](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、伺服器端、混合式) |
| pollingInterval | 數字 | 無 | 300000 （5分鐘） | 的輪詢間隔 [裝置上決策規則成品](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) （以毫秒為單位） |
| artifectlocation | 字串 | 無 | 無 | 的完整URL [裝置上決策規則成品](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). 覆寫內部決定的位置。 |
| artifactPayload | 物件 | 無 | 無 | 的JSON裝載 [裝置上決策規則成品](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). 若指定，會加以使用，而非向URL要求。 |
| [events](sdk-events.md) | 物件&lt;string function=&quot;&quot;> | 否 | 無 | 具有事件名稱索引鍵和回呼函式值的選用物件 |
| telemetryEnabled | 布林值 | 無 | true | 啟用後，Adobe將會收集SDK功能使用情況和效能遙測資料。 不會收集個人資料。 |

## 範例

### Node.js

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    events: {clientReady: targetClientReady }
};

const targetClient = TargetClient.create(CONFIG);

function targetClientReady() {
    // make calls to Adobe Target
}
```
