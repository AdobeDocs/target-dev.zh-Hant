---
keywords: 全域mbox引數， targetPageParams，查詢字串，陣列， json， dtm
description: 瞭解如何使用 [!UICONTROL targetPageParams] 函式來傳遞其他目標定位或內容資訊至 [!DNL Adobe Target] 全域mbox。
title: 如何將引數傳遞至全域mbox？
feature: at.js
exl-id: 2a6be3e4-a618-4812-9e87-b01789705c40
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 63%

---

# 傳遞參數至全域 mbox

JavaScript `targetPageParams` 函式用於將引數傳遞至中的全域mbox [!DNL Adobe Target]. 在任何要傳遞其他目標/內容資訊的情境中，都需要此專案 [!DNL Target].

例如，在「建議」活動中，使用參數來代表目前正在檢視的產品或類別。

呼叫JavaScript函式的程式碼必須位於頁面上的全域mbox之前，無論全域mbox是作為at.js的一部分引發，還是手動納入頁面程式碼中。

>[!NOTE]
>
>如果您想要將引數新增至頁面上的所有mbox，而不只是新增至全域mbox，請使用 [targetPageParamsAll()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) 函式。

您可以透過下列任何方式，利用 `target-global-mbox` 函數將參數傳入 `targetPageParams()`:

* 陣列
* JSON 物件
* 以 &amp; 符號區隔的清單

使用這三個方法來驗證是否正確傳遞參數。您可以使用 [Adobe Experience Cloud Debugger](https://experienceleague.adobe.com/docs/debugger/using/experience-cloud-debugger.html) 來驗證參數傳遞。

在將全域 mbox 新增至頁面之前，您必須定義 JavaScript 函式。名稱必須是 `targetPageParams`。

## 查詢字串

```
p1=v1&p2=v2&p3=hello%20world
```

* 名稱: `targetPageParams`
* 傳回值: 以 &quot;&amp;&quot; 分隔的參數，含 URL 編碼的參數值。

  範例:  

  在此範例中，p3 的值是 `hello world` (以 URL 編碼)。

考量下列範例頁面程式碼：

```html {line-numbers="true"}
<html> 
  <head> 
    <title>Title here..</title> 
    <script type="text/javascript"> 
        function targetPageParams() { 
          return "p1=v1&p2=v2&p3=hello%20world";
        } 
    </script> 
    <script src="mbox.js" type="text/javascript"></script> 
  </head> 
  <body>Body here... 
  </body> 
</html>
```

此範例會將下列資料傳送至 mbox 邊緣:

* p1=v1
* p2=v2
* p3=hello world

## 陣列

```javascript {line-numbers="true"}
<!--window.-->targetPageParams = function() { 
  return ["a=1", "b=2", "c=hello world"]; 
}; 
```

值不需經過 URL 編碼。例如，假設一個值包含空格，則不需要將空格編碼。

此範例會將下列資料傳送至 mbox 邊緣:

* a=1
* b=2
* c=hello world

## JSON

JSON 是傳遞參數的強大方式。[!DNL Target] 使用 JSON 物件索引鍵將複雜的結構平扁化為簡單參數。

```json {line-numbers="true"}
<!--window.-->targetPageParams = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
                  "memberStatus": Gold, 
                  "country": { 
                                "city": "San Francisco" 
                            } 
              } 
  }; 
}; 
```

值不需經過 URL 編碼。例如，&quot;San Francisco&quot; 不需要將空格編碼。一個空格就足夠。

此範例會將下列資料傳送至 mbox 邊緣:

* a=1
* b=2
* `profile.memberStatus`=金級
* `profile.country.city`=San Francisco
