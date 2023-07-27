---
keywords: targetPageParams， targetpageparams， pageParams， pageparams，頁面引數，頁面引數， at.js，函式，函式， targetPageParams0
description: 使用 [!UICONTROL targetPageParams()] 的函式 [!DNL Adobe Target] at.js JavaScript程式庫，可從要求程式碼外部將引數附加至全域mbox。
title: 如何使用 [!UICONTROL targetPageParams()] 功能？
feature: at.js
exl-id: 274e4d1f-843a-443b-ad98-7139dc4a13f8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 65%

---

# [!UICONTROL targetPageParams()]

此方法允許您將參數從要求程式碼外部附加至全域 mbox。

對於要在多個 mbox 呼叫上併入相同的一組參數，此函數很實用。函數需要由客戶定義。它應該傳回僅會傳遞至全域 mbox 要求的參數陣列。此函式可在at.js載入前或其中定義 **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 編輯]** > **[!UICONTROL 資料庫標題]**.

您可以使用 `[!UICONTROL targetPageParams()]` 函數，透過下列任何方式將參數傳入 target-global-mbox 中:

* 以 &amp; 符號區隔的清單
* 陣列
* JSON 物件

## 範例

以 &amp; 符號分隔的清單 (值必須經過 URL 編碼):

```javascript {line-numbers="true"}
function targetPageParams() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

陣列 (值不需經過 URL 編碼):

```javascript {line-numbers="true"}
targetPageParams = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON (值不需經過 URL 編碼):

```javascript {line-numbers="true"}
targetPageParams = function() { 
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
