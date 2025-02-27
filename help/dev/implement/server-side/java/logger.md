---
title: 初始化 [!DNL Adobe Target] Java SDK以記錄要求
description: 瞭解如何在 [!DNL Adobe Target] Java SDK中記錄請求。
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
source-git-commit: 526445fccee9b778b7ac0d7245338f235f11d333
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 4%

---

# Logger (Java)

## 說明

當[初始化SDK](initialize-sdk.md)時，`ClientConfig`物件上有數個選項，這些選項可以設定為記錄要求。

| 選項 | 說明 |
| --- | --- |
| `logRequests` | 記錄整個要求內文及回應內文。 |
| `logRequestStatus` | 記錄請求的url、狀態以及回應時間。 |

[!DNL Target] Java SDK使用`slf4j`記錄。 您必須提供記錄器的實作，例如`java.util.logging`、`logback`和`log4j`。 如需詳細資訊，請參閱[https://www.slf4j.org/manual.html](https://www.slf4j.org/manual.html)。 所有記錄都將在`debug`中列印。

## 範例

新增`slf4j`相依性。

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

根據您的實作啟用`DEBUG`記錄，並標示要求記錄旗標。

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
