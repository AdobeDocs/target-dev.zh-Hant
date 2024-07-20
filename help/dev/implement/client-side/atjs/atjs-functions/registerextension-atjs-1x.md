---
keywords: registerExtension， registerextension，註冊擴充功能， at.js，函式，函式， clientCode， serverDomain， globalMboxName， globalMboxAutoCreate， timeout， registerExtension2
description: 使用適用於 [!DNL Adobe Target] at.js JavaScript程式庫的[!UICONTROL registerExtension()]函式來註冊特定的延伸模組。 (at.js 1.x)
title: 如何使用[!UICONTROL registerExtension()]函式？
feature: at.js
exl-id: 71decf00-84c5-4914-b0cd-bb061fa6265f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 69%

---

# [!UICONTROL registerExtension()] - at.js 1.x

提供標準方式來註冊特定的延伸模組。

>[!NOTE]
>
>此函數僅適用於 at.js 1.*x* 版。此函式已在at.js 2版本中棄用。*x* 使用供跨網域追蹤功能時。 如果與at.js 2.x搭配使用，此函式會傳回預設內容。

options 參數是強制性的，並且具有下列結構:

| 索引鍵 | 類型 | 必要 | 說明 |
|--- |--- |--- |--- |
| 名稱 | 字串 | 是 | 延伸模組名稱。 |
| 模組 | Array[String] | 是 | 代表要求的模組名稱的字串陣列。 |
| register | 函數 | 是 | 用來初始化和建置延伸模組的函數。此函數會根據模組陣列接收引數。 |

附註:

* 如果未提供其中一個參數，則會擲出例外。
* 如果模組陣列是空的，則會擲出例外。

如需詳細資訊和如何使用`[!UICONTROL registerExtension]`的範例，請參閱GitHub上的[Adobe Experience Cloud Target atjs擴充功能](https://github.com/Adobe-Marketing-Cloud/target-atjs-extensions)頁面。

## 設定模組方法

| 索引鍵 | 類型 | 說明 |
|--- |--- |--- |
| clientCode | 字串 | 用戶端代碼 |
| serverDomain | 字串 | Edge 伺服器網域 |
| globalMboxName | 字串 | [!DNL Target]全域mbox名稱 |
| globalMboxAutoCreate | 布林值 | 指出自動建立是否已啟用 |
| timeout | 數字 | 要求逾時 |

## 記錄器模組方法

| 索引鍵 | 類型 | 說明 |
|--- |--- |--- |
| 記錄 | 函數 | 將引數的變數清單記錄至瀏覽器主控台 (如果存在)。只有在將 `mboxDebug=true` 傳遞至 URL 時，它才會啟動。 |
| error | 函數 | 將引數的變數清單記錄至瀏覽器主控台。只有在有嚴重錯誤，例如網路逾時、找不到 HTML 節點時，它才會啟動。 |
