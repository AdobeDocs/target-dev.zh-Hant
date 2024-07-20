---
title: 瞭解如何設定自訂HTTP使用者端
description: 瞭解如何使用ClientConfig.builder()。httpClient()設定TargetClient。
feature: APIs/SDKs
exl-id: 7615029c-b62d-4ed1-aadb-32e364c4c654
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 0%

---

# 自訂HTTP使用者端設定(Java)

如果執行SDK的應用程式需要自訂HTTP使用者端，若要啟用設定SSL或新增預設標頭至要求等功能，則需要使用`ClientConfig.builder().httpClient()`設定`TargetClient`：

## 基本自訂HTTP使用者端設定

SDK目前支援實作`org.apache.http.client.HttpClient`介面的HTTP使用者端。

### 基本實施

```java {line-numbers="true"}
CloseableHttpClient httpClient = HttpClients.custom().build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## 使用SSL設定自訂HTTP使用者端設定

以下範例說明如何透過自訂傳遞至`ClientConfig`的`HttpClient`，在`TargetClient`中設定SSL。 下列程式碼片段使用`org.apache.http.conn.ssl`封裝中的類別進行SSL設定。

### SSL實施

```java {line-numbers="true"}
SSLContext context = SSLContextBuilder.create().build();
SSLConnectionSocketFactory sslSocketFactory = new SSLConnectionSocketFactory(context);
CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(sslSocketFactory).build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```
