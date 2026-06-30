---
keywords: adobe.target.applyOffer， applyOffer， applyoffer，套用選件， at.js，函式，函式， $8
description: 使用適用於 [!DNL Adobe Target] at.js JavaScript資料庫的[!UICONTROL adobe.target.applyOffer()]函式來套用回應內容。
title: 如何使用[!UICONTROL adobe.target.applyOffer()]函式？
feature: at.js
exl-id: 957bbe92-8012-4bd5-95d6-1ae38b72bb16
TQID: https://experienceleague.adobe.com/lrjsIl-gKu1SnrZapxYcoDObvUCGG2ht58QtWbQkYts
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d851e2344279caeae25e4823ca86b9c17efd63
workflow-type: tm+mt
source-wordcount: 173
ht-degree: 68%

---

# [!UICONTROL adobe.target.applyOffer(options)]

此函式用於套用[!DNL Adobe Target]的回應內容。

>[!NOTE]
>
>`applyOffer` 需要 `mbox` 參數。 如果未指定 mbox 名稱，則會發生錯誤。

options 參數是強制性的，並且具有下列結構:

| 索引鍵 | 類型 | 必要 | 說明 |
|--- |--- |--- |--- |
| mbox | 字串 | 是 | mbox 名稱<br />利用 at.js 1.3.0 (和更新版本)，Target 可強制使用 mbox 機碼。 此機碼在過去為必要，但現在 Target 會強制使用它，以確保 Target 有正確的驗證，且客戶能正確使用函數。 |
| selector | 字串或 DOM 元素 | 無 | HTML 元素或 CSS 選取器過去會識別 Target 應該放置產品建議內容的 HTML 元素。 如果未提供選取器，Target會假設HTML元素應使用HTML HEAD。 |
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


