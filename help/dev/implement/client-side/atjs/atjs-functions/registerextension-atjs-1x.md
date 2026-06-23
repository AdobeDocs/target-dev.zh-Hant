---
keywords: registerExtension， registerextension，註冊擴充功能， at.js，函式，函式， clientCode， serverDomain， globalMboxName， globalMboxAutoCreate， timeout， registerExtension2
description: 使用適用於 [!DNL Adobe Target] at.js JavaScript程式庫的[!UICONTROL registerExtension()]函式來註冊特定的延伸模組。 (at.js 1.x)
title: 如何使用[!UICONTROL registerExtension()]函式？
feature: at.js
exl-id: 71decf00-84c5-4914-b0cd-bb061fa6265f
TQID: https://experienceleague.adobe.com/qTWubp0dNesN-8vsooz8pdbjfSw1W1ktm-0bG6YRzJw
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 277
ht-degree: 62%

---

# [!UICONTROL registerExtension()] - at.js 1.x

提供標準方式來註冊特定的延伸模組。

>[!NOTE]
>
>此函式僅適用於at.js 1.*x*&#x200B;版。 此函式已在at.js 2.*x*&#x200B;版本中棄用。 如果與at.js 2.x搭配使用，此函式會傳回預設內容。

options 參數是強制性的，並且具有下列結構:

| 索引鍵 | 類型 | 必要 | 說明 |
|--- |--- |--- |--- |
| 名稱 | 字串 | 是 | 延伸模組名稱。 |
| 模組 | Array[String] | 是 | 代表要求的模組名稱的字串陣列。 |
| register | 函數 | 是 | 用來初始化和建置延伸模組的函數。 此函數會根據模組陣列接收引數。 |

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
| 記錄 | 函數 | 將引數的變數清單記錄至瀏覽器主控台 (如果存在)。 只有在將 `mboxDebug=true` 傳遞至 URL 時，它才會啟動。 |
| error | 函數 | 將引數的變數清單記錄至瀏覽器主控台。 只有在有嚴重錯誤，例如網路逾時、找不到 HTML 節點時，它才會啟動。 |

