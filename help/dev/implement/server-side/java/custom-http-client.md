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

如果執行SDK的應用程式需要自訂HTTP使用者端，若要啟用設定SSL或新增預設標頭至請求等功能，則 `TargetClient` 需要使用進行設定 `ClientConfig.builder().httpClient()`：

## 基本自訂HTTP使用者端設定

SDK目前支援實作的HTTP使用者端 `org.apache.http.client.HttpClient` 介面。

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

以下是如何在中設定SSL的範例 `TargetClient` 透過自訂 `HttpClient` 傳遞至 `ClientConfig`. 下列程式碼片段使用的類別來自 `org.apache.http.conn.ssl` SSL設定的套件。

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
