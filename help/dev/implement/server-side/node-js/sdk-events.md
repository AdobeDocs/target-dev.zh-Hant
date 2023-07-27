---
title: 訂閱中的事件 [!DNL Adobe Target] Node.js SDK
description: 瞭解如何使用，訂閱Node.js SDK中發生的各種事件。 [!UICONTROL OnDeviceDecisioningHandler] 物件。
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 2%

---

# SDK事件(Node.js)

## 說明

時間 [初始化SDK](initialize-sdk.md)，則 `options.events` object是選用的物件，具有事件名稱索引鍵和回呼函式值。 可用來訂閱SDK內發生的各種事件。 例如 `clientReady` 事件可與回呼函式搭配使用，當SDK準備好進行方法呼叫時，系統會叫用該回呼函式。

呼叫回呼函式時，事件物件會傳入。 每個事件都有一個 `type` 與事件名稱相對應。 有些事件包含其他屬性以及相關資訊。

## 事件

| 事件名稱（型別） | 說明 | 其他事件屬性 |
| --- | --- | --- |
| clientready | 已在下載成品且SDK已就緒時發出 `getOffers` 呼叫。 使用裝置上決策方法時建議使用。 |
| artifactDownloadSucceeded | 每次下載新成品時發出。 | artifactPayload， artifactLocation |
| artifactDownloadFailed | 每當成品無法下載時發出。 | artifactLocation，錯誤 |

## 範例

### Node.js

```js {line-numbers="true"}
const targetClient = TargetClient.create({
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device",
    events: {
        clientReady: onTargetClientReady,
        artifactDownloadSucceeded: onArtifactDownloadSucceeded,
        artifactDownloadFailed: onArtifactDownloadFailed
    }
});

function onTargetClientReady() {
    // make getOffers requests
    targetClient.getOffers({...})            
}

function onArtifactDownloadSucceeded(event) {
    console.log(`The artifact was successfully downloaded from '${event.artifactLocation}'`);
    // optionally do something with event.artifactPayload, like persist it
}

function onArtifactDownloadFailed(event) {
    console.log(`The artifact failed to download from '${event.artifactLocation}' with the following error message: ${event.error.message}`);
}
```
