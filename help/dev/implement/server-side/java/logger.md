---
title: 初始化 [!DNL Adobe Target] 記錄請求的Java SDK
description: 瞭解如何在中記錄請求 [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 4%

---

# Logger (Java)

## 說明

時間 [初始化SDK](initialize-sdk.md)中，有數個選項可以 `ClientConfig` 物件，可設定為記錄請求。

| 選項 | 說明 |
| --- | --- |
| `logRequests` | 記錄整個要求內文及回應內文。 |
| `logRequestStatus` | 記錄請求的url、狀態以及回應時間。 |

[!DNL Target] Java SDK使用 `slf4j` 記錄。 您需要提供記錄器的實作，例如 `java.util.logging`， `logback`、和 `log4j`. 請參閱 [http://www.slf4j.org/manual.html](http://www.slf4j.org/manual.html) 以取得詳細資訊。 所有記錄都將列印在 `debug`.

## 範例

新增 `slf4j` 相依性。

>[!BEGINTABS]

>[!TAB Gradle]

### Gradle

```javascript {line-numbers="true"}
compile 'org.slf4j:slf4j-simple:2.0.0-alpha0'
```

>[!TAB Maven]

```javascript {line-numbers="true"}
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>2.0.0-alpha0</version>
</dependency>
```

>[!ENDTABS]

啟用 `DEBUG` 會根據您的實作來記錄下來，並標示請求記錄旗標。

### 除錯

```javascript {line-numbers="true"}
System.setProperty(SimpleLogger.DEFAULT_LOG_LEVEL_KEY, "DEBUG");
ClientConfig config = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .logRequests(true)
        .logRequestStatus(true)
        .build();

TargetClient targetClient = TargetClient.create(config);
```

您應該會看到主控台中列印的請求、回應和回應時間。
