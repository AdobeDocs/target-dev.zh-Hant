---
title: 訂閱 [!DNL Adobe Target] Java SDK中的事件
description: 瞭解如何使用[!UICONTROL OnDeviceDecisioningHandler]物件來訂閱Java SDK中發生的各種事件。
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
TQID: https://experienceleague.adobe.com/x3aig-jM-GXzmLNcUNclZUK9Y49tuSF9-sdkxzJFtiM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 134
ht-degree: 5%

---

# SDK Events (Java)

## 說明

當[初始化SDK](initialize-sdk.md)時，可在`ClientConfig`物件上提供選用的`OnDeviceDecisioningHandler`物件。 它可用來訂閱SDK內發生的各種事件。 例如，`onDeviceDecisioningReady`事件可與回呼函式搭配使用，當SDK準備好進行方法呼叫時，將會叫用該回呼函式。

## 事件

`OnDeviceDecisioningHandler`物件包含下列為特定事件呼叫的回呼：

| 名稱 | 引數 | 說明 |
| --- | --- | --- |
| DevicedecisioningReady | 無 | 使用者端第一次準備好[!UICONTROL on-device decisioning]時只呼叫一次 |
| artifactDownloadSucceeded | 成品檔案的位元組[]內容 | 每次下載[!UICONTROL on-device decisioning]成品時呼叫 |
| artifactDownloadFailed | 例外 | 每次無法下載[!UICONTROL on-device decisioning]成品時呼叫 |

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
