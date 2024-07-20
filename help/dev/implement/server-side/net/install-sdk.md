---
title: 安裝.NET SDK
description: 瞭解如何安裝 [!DNL Adobe Target] .NET SDK。
feature: APIs/SDKs
exl-id: 3cc84775-4692-4d14-9e82-db2873140835
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# 安裝.NET SDK

.NET SDK由[NuGet](https://www.nuget.org/packages/Adobe.Target.Client)發佈。 若要開始，請透過`Package Manage`或`.NET CLI`安裝，將它新增為相依性：

## 封裝管理員

>[!BEGINTABS]

>[!TAB 封裝管理員]

```csharp {line-numbers="true"}
Install-Package Adobe.Target.Client
```

>[!TAB .NET CLI]

```csharp {line-numbers="true"}
dotnet add package Adobe.Target.Client
```

>[!ENDTABS]

您可以在[https://github.com/adobe/target-dotnet-sdk](https://github.com/adobe/target-dotnet-sdk)找到開放原始程式碼。
