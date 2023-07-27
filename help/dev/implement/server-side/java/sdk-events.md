---
title: 訂閱中的事件 [!DNL Adobe Target] Java SDK
description: 瞭解如何使用訂閱Java SDK中發生的各種事件。 [!UICONTROL OnDeviceDecisioningHandler] 物件。
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 4%

---

# SDK事件(Java)

## 說明

時間 [初始化SDK](initialize-sdk.md)，選填 `OnDeviceDecisioningHandler` 物件可在 `ClientConfig` 物件。 可用來訂閱SDK內發生的各種事件。 例如， `onDeviceDecisioningReady` 事件可與回呼函式搭配使用，當SDK準備好進行方法呼叫時，系統會叫用該回呼函式。

## 事件

此 `OnDeviceDecisioningHandler` 物件包含下列為特定事件呼叫的回呼：

| 名稱 | 引數 | 說明 |
| --- | --- | --- |
| DevicedecisioningReady | 無 | 使用者端第一次準備就緒時只呼叫一次 [!UICONTROL 裝置上決策] |
| artifactDownloadSucceeded | 位元組[] 成品檔案的內容 | 每次 [!UICONTROL 裝置上決策] 成品已下載 |
| artifactDownloadFailed | 例外 | 每次無法下載時呼叫 [!UICONTROL 裝置上決策] 成品 |

## 範例

### SDK事件

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .defaultDecisioningMethod(DecisioningMethod.ON_DEVICE)
        .onDeviceDecisioningHandler(new OnDeviceDecisioningHandler() {
            @Override
            public void onDeviceDecisioningReady() {
                // make getOffers requests
                makeTargetRequests();
            }

            @Override
            public void artifactDownloadSucceeded(byte[] artifactData) {
                System.out.println("The artifact was successfully downloaded.");
            }

            @Override
            public void artifactDownloadFailed(TargetClientException e) {
                System.out.println("The artifact failed to download.");
            }
        }).build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);


void makeTargetRequests() {
    List<MboxRequest> mboxRequests = new ArrayList<>();
    mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

    TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
            .context(new Context().channel(ChannelType.WEB))
            .execute(new ExecuteRequest().setMboxes(mboxRequests))
            .build();

    TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
}
```
