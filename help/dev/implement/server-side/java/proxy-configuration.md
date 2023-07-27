---
title: 在中實作Proxy設定 [!DNL Adobe Target] Java SDK
description: 瞭解如何在中設定TargetClient proxy設定 [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 1%

---

# Proxy設定(Java)

## 基本代理伺服器

如果執行SDK的應用程式需要Proxy才能存取網際網路，則 `TargetClient` 將需使用Proxy設定進行設定，如下所示。

### 基本Proxy設定

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## 驗證

如果需要Proxy驗證，則認證可作為引數傳遞至 `ClientProxyConfig` 建構函式，如以下範例所示。 請注意，這僅適用於簡易的使用者名稱/密碼Proxy驗證。

### 基本Proxy驗證

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port,username,password))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```
