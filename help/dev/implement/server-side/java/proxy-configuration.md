---
title: 在 [!DNL Adobe Target] Java SDK中實作Proxy設定
description: 瞭解如何在 [!DNL Adobe Target] Java SDK中設定TargetClient Proxy設定。
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
TQID: https://experienceleague.adobe.com/Vo8KrM-3AGIvoO-E-iAQcAPqzXE24BM30LX7ji5E2Nk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 170
ht-degree: 2%

---

# Proxy設定(Java)

## 基本代理伺服器

如果執行SDK的應用程式需要Proxy來存取網際網路，則`TargetClient`必須設定如下Proxy設定。

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

如果需要Proxy驗證，則認證可作為引數傳遞給`ClientProxyConfig`建構函式，如以下範例所示。 請注意，這僅適用於簡易的使用者名稱/密碼Proxy驗證。

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

對於擷取規則成品的請求，應將Proxy設定為不快取回應。 不過，如果無法針對該要求設定Proxy的快取機制，請使用設定選項作為繞過Proxy層級快取的因應措施。 此因應措施會將字串值空白的`Authorization`標頭新增至規則要求，以指示Proxy不應快取回應。

若要啟用此因應措施，請設定下列專案：

```java {line-numbers="true"}
ClientConfig.builder()
    .shouldArtifactRequestBypassProxyCache(true)
    .build();
```


