---
title: 在中實作Proxy設定 [!DNL Adobe Target] Java SDK
description: 瞭解如何在中設定TargetClient proxy設定 [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: 59ab3f53e2efcbb9f7b1b2073060bbd6a173e380
workflow-type: tm+mt
source-wordcount: '170'
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

## 裝置上決策

對於擷取規則成品的請求，應將Proxy設定為不快取回應。 不過，如果無法針對該要求設定Proxy的快取機制，請使用設定選項作為繞過Proxy層級快取的因應措施。 此因應措施新增 `Authorization` 帶有空字串值的標頭傳送至規則請求，應向Proxy指出不應快取回應。

若要啟用此因應措施，請設定下列專案：

```java {line-numbers="true"}
ClientConfig.builder()
    .shouldArtifactRequestBypassProxyCache(true)
    .build();
```


