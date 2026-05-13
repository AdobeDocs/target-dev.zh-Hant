---
title: 安裝Java SDK
description: 瞭解如何安裝 [!DNL Adobe Target] Java SDK。
feature: APIs/SDKs
exl-id: 5828d5b3-c487-49bf-9458-7ef94374e32d
TQID: https://experienceleague.adobe.com/NJ8oBLe6fuxcU67YGBg8dFFg32YzBX2jB1HNFegcaGk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 51
ht-degree: 0%

---

# 安裝Java SDK

Java SDK由[Maven Central](https://search.maven.org/artifact/com.adobe.target/target-java-sdk)發佈。 若要開始，請透過在`gradle`或`maven`中安裝，將其新增為相依性：

>[!BEGINTABS]

>[!TAB Gradle]

```javascript {line-numbers="true"}
compile 'com.adobe.target:java-sdk:1.0'
```

>[!TAB Maven]

```javascript {line-numbers="true"}
<dependency>
    <groupId>com.adobe.target</groupId>
    <artifactId>java-sdk</artifactId>
    <version>2.0</version>
</dependency>
```

>[!ENDTABS]

您可以在<https://github.com/adobe/target-java-sdk>找到開放原始碼。
