---
keywords: targetPageParamsAll， targetpageparamsall， PageParamsAll， pageparamsall，頁面引數，頁面引數， at.js，函式，函式， targetPageParamsAll0
description: 使用 [!UICONTROL targetPageParamsAll()] 的函式 [!DNL Adobe Target] at.js JavaScript程式庫，可將引數從要求程式碼外部附加至所有mbox。
title: 如何使用 [!UICONTROL targetPageParamsAll()] 功能？
feature: at.js
exl-id: 32045e60-6904-42a1-bf71-fd7e167a829f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 55%

---

# [!UICONTROL targetPageParamsAll()]

此方法允許您將參數從要求程式碼外部附加至所有 mbox。

對於要在多個 mbox 呼叫上併入相同的一組參數，這很實用。函數需要由客戶定義。它應該傳回將傳遞至頁面上所有 mbox 要求的參數陣列。此函式可在at.js載入前或其中定義 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 編輯]** > **[!UICONTROL 程式碼設定]** > **[!UICONTROL 資料庫標題]**.

您可以使用以下將引數傳入target-global-mbox： [!UICONTROL targetPageParamsAll()] 功能為下列任一方式：

* 以 &amp; 符號區隔的清單
* 陣列
* JSON 物件

## 範例

以 &amp; 符號分隔的清單 (值必須經過 URL 編碼):

```javascript {line-numbers="true"}
function targetPageParamsAll() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

陣列 (值不需經過 URL 編碼):

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (值不需經過 URL 編碼):

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
        "age": 26, 
        "country": { 
          "city": "San Francisco" 
        } 
      } 
  }; 
};
```
