---
title: 訂閱 [!DNL Adobe Target] .NET SDK中的事件
description: 瞭解如何使用[!UICONTROL OnDeviceDecisioningHandler]物件來訂閱.NET SDK中發生的各種事件。
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 5%

---

# SDK事件(.NET)

## 說明

當[初始化SDK](initialize-sdk.md)時，可在`TargetClientConfig`物件上提供選用的`OnDeviceDecisioningReady`委派，當SDK準備好進行裝置上方法呼叫時，將會叫用該委派。 還有一些其他代理人可處理[!UICONTROL on-device decisioning]成品下載。

## 事件

可以為某些事件設定以下委派：

| 名稱 | 引數 | 說明 |
| --- | --- | --- |
| OndevicedecisioningReady | 無 | 使用者端第一次準備好[!UICONTROL on-device decisioning]時只呼叫一次 |
| ArtifactDownloadSucceeded | 成品檔案的字串內容 | 每次下載[!UICONTROL on-device decisioning]成品時呼叫 |
| ArtifactDownloadFailed | 例外 | 每次無法下載[!UICONTROL on-device decisioning]成品時呼叫 |

## 範例

### \.NET

```dotnet {line-numbers="true"}
var clientConfig = new TargetClientConfig.Builder("acmeclient", "1234567890@AdobeOrg")
    .SetDecisioningMethod(DecisioningMethod.OnDevice)
    .SetOnDeviceDecisioningReady(DecisioningReady)
    .SetArtifactDownloadSucceeded(artifact => Console.WriteLine("The artifact was successfully downloaded. Contents: " + artifact))
    .SetArtifactDownloadFailed(exception => Console.WriteLine("The artifact failed to download. Exception: " + exception.Message))
    .Build();

var targetClient = TargetClient.Create(clientConfig);

// ...

static void DecisioningReady()
{
    var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

    var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
        .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
        .Build();

    var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
}
```
