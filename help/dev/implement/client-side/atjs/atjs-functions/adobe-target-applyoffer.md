---
keywords: adobe.target.applyOffer， applyOffer， applyoffer，套用選件， at.js，函式，函式， $8
description: 對 [!DNL Adobe Target] at.js JavaScript資料庫使用[!UICONTROL adobe.target.applyOffer()]函式以套用回應內容。
title: 如何使用[!UICONTROL adobe.target.applyOffer()]函式？
feature: at.js
exl-id: 957bbe92-8012-4bd5-95d6-1ae38b72bb16
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 69%

---

# [!UICONTROL adobe.target.applyOffer(options)]

此函式用於套用[!DNL Adobe Target]的回應內容。

>[!NOTE]
>
>`applyOffer` 需要 `mbox` 參數。如果未指定 mbox 名稱，則會發生錯誤。

options 參數是強制性的，並且具有下列結構:

| 索引鍵 | 類型 | 必要 | 說明 |
|--- |--- |--- |--- |
| mbox | 字串 | 是 | mbox 名稱<br />利用 at.js 1.3.0 (和更新版本)，Target 可強制使用 mbox 機碼。此機碼在過去為必要，但現在 Target 會強制使用它，以確保 Target 有正確的驗證，且客戶能正確使用函數。 |
| selector | 字串或 DOM 元素 | 無 | HTML 元素或 CSS 選取器過去會識別 Target 應該放置選件內容的 HTML 元素。如果未提供選取器，Target會假設HTML元素應使用HTMLHEAD。 |
| 產品建議 | 陣列 | 是 | 應該套用至元素的動作陣列。 |

## 範例

下列範例顯示如何將 `[!UICONTROL getOffer]` 和 `[!UICONTROL applyOffer]` 一起使用:

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "mbox",   
  "success": function(offers) {           
        adobe.target.applyOffer( {  
           "mbox": "mbox", 
           "offer": offers  
        } ); 
  },   
  "error": function(status, error) {           
      if (console && console.log) { 
        console.log(status); 
        console.log(error); 
      } 
  }, 
 "timeout": 5000 
}); 
```
