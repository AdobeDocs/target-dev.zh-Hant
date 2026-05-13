---
title: 如何在 [!DNL Adobe Target] .NET SDK中使用非同步要求
description: 瞭解 [!DNL Target] Java SDK如何支援非同步要求，將有效目標時間減少為零。
feature: APIs/SDKs
exl-id: fd36cc7b-a884-4e57-93c2-8aff8256109a
TQID: https://experienceleague.adobe.com/E9rNmPdXe7HYg7XlIffpC4opGM9X6fFoHK-u0oLI-XE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 91
ht-degree: 4%

---

# 非同步要求(.NET)

## 說明

伺服器端整合的好處之一，就是可以利用伺服器端可用的大量頻寬和運算資源，透過使用平行功能。[!DNL Target] .NET SDK支援非同步要求，可輕鬆將[!DNL Target]整合至應用程式現有的非同步工作流程。

## 支援的方法

### \.NET

```dotnet {line-numbers="true"}
Task<TargetDeliveryResponse> GetOffersAsync(TargetDeliveryRequest request);
Task<TargetDeliveryResponse> SendNotificationsAsync(TargetDeliveryRequest request);
Task<TargetAttributes> GetAttributesAsync(TargetDeliveryRequest request, params string[] mboxes);
```

## 範例

非同步SDK API使用方式的範例可能如下所示：

### \.NET

```dotnet {line-numbers="true"}
var deliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: new List<MboxRequest> { new MboxRequest(index: 1, name: "a1-serverside-ab") }))
    .Build();

var response = await this.targetClient.GetOffersAsync(deliveryRequest);

var notificationRequest = new TargetDeliveryRequest.Builder()
    .SetSessionId(response.Request.SessionId)
    .SetTntId(response.Response?.Id?.TntId)
    .SetNotifications(new List<Notification>
        {
            new (id: "1", type: MetricType.Display, timestamp: DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
                mbox: new NotificationMbox("product1", "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"),
                tokens: new List<string> { "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" })
        })
    .Build();

var notificationResponse = await this.targetClient.SendNotificationsAsync(notificationRequest);
```

此範例假設您已初始化[SDK](initialize-sdk.md)。
