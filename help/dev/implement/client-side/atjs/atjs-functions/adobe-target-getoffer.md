---
keywords: adobe.target.getOffer， getOffer， getoffer， get offer， at.js，函式，函式， $8
description: 使用[!UICONTROL adobe.target.getOffer()]函式及其選項讓 [!DNL Adobe Target] at.js程式庫觸發要求，以取得 [!DNL Target] 選件。
title: 如何使用[!UICONTROL adobe.target.getOffer()]函式？
feature: at.js
exl-id: 7b917d42-06e8-4838-a09d-0c4872c9beaa
TQID: https://experienceleague.adobe.com/GcXVIt-42-PV0j4Q4oe5uePTZAn7PDIMicIAULDXz-s
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 472
ht-degree: 71%

---

# [!DNL adobe.target.getOffer(options)]

此函式會引發要求以取得[!DNL Target]選件。

搭配使用 `[!UICONTROL adobe.target.applyOffer()]` 來處理回應或使用您自己的成功處理。 options 參數是強制性的，並且具有下列結構:

| 索引鍵 | 類型 | 必要 | 說明 |
|--- |--- |--- |--- |
| mbox | 字串 | 是 | Mbox 名稱 |
| params | 物件 | 無 | mbox 參數. 機碼/值組的物件具有下列結構:<P>`{ "param1": "value1", "param2": "value2"}` |
| success | 函數 | 是 | 收到來自伺服器的回應時要執行的回呼。 success 回呼函數將接收代表產品建議物件陣列的單一參數。 以下是success回呼範例：<P>`function handleSuccess(response){......}`<P>請參閱以下的「回應」以取得詳細資料。 |
| error | 函數 | 是 | 遇到錯誤時要執行的回呼。 有一些案例會被視為錯誤:<ul><li>HTTP 狀態代碼與 200 OK 不同</li><li>無法剖析回應。 例如，我們拙劣地建構了 JSON 或 HTML 而非 JSON。</li><li>回應包括 &quot;error&quot; 索引鍵。 例如，在 Edge 上擲出了例外，無法正確處理要求。 當mbox遭到封鎖，我們無法擷取任何內容時，便可能出現錯誤，諸如此類情況。error回呼函式將收到兩個引數： status和error。 以下是錯誤回呼範例： `function handleError(status, error){......}`</li></ul>請參閱以下的「錯誤回應」以取得詳細資料。 |
| timeout | 數字 | 無 | 逾時，以毫秒為單位。 如果未指定，將使用 at.js 中的預設逾時。<P>可以從[!UICONTROL 管理] > [!UICONTROL 實作]下的[!DNL Target] UI設定預設逾時。 |

## 範例

正在使用[!UICONTROL getOffer()]新增引數，並使用[!UICONTROL applyOffer()]進行成功處理：

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

正在新增引數和[!UICONTROL getOffer()]的設定檔引數，並使用[!UICONTROL applyOffer()]進行成功處理：

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2, 
     "profile.age": 27, 
     "profile.gender": "male" 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

搭配[!UICONTROL getOffer()]使用自訂逾時和自訂成功處理：

&quot;YOUR_OWN_CUSTOM_HANDLING_FUNCTION&quot; 客戶會定義的函數之預留位置。

```javascript {line-numbers="true"}
adobe.target.getOffer({     
  "mbox": "target-global-mbox",   
  "success": function(offer) { 
    YOUR_OWN_CUSTOM_HANDLING_FUNCTION(offer);   
  }, 
  "error": function(status, error) {                 
    console.log('Error', status, error);   
  },   
  "timeout": 2000 
});
```

## 回應

傳遞至 success 回呼的回應參數將會是動作的陣列。 動作是一種物件，通常具備下列格式:

| 名稱 | 類型 | 說明 |
|--- |--- |--- |
| 動作 | 字串 | 要套用至識別的元素的動作類型。 |
| selector | 字串 | 代表 Sizzle 選取器。 |
| cssSelector | 字串 | DOM 原生選取器，用於元素預先隱藏。 |
| 內容 | 字串 | 要套用至識別的元素的內容。 |

## 範例

```javascript {line-numbers="true"}
{ 
    "sessionId": "1444512212156-384616", 
    "tntId": "1444512212156-384616.17_35", 
    "offers": [{ 
        "plugins": ["<script type=\"text/javascript\">\r\n/*mboxHighlight+ (1of2) v1 ==> Response Plugin*/\r\nwindow.ttMETA=(typeof(window.ttMETA)!='undefined')?window.ttMETA:[];window.ttMETA.push({'mbox':'target-global-mbox','campaign':'at: redirect ootb','experience':'Experience B','offer':'/at_redirect_ootb/experiences/1/pages/0/1442082890250'});window.ttMBX=function(x){var mbxList=[];for(i=0;i<ttMETA.length;i++){if(ttMETA[i].mbox==x.getName()){mbxList.push(ttMETA[i])}}return mbxList[x.getId()]}\r\n</script>"], 
        "actions": { 
            "content": [{ 
                "passMboxSession": false, 
                "selector": "body", 
                "action": "redirect", 
                "url": "https://example.com/04.html", 
                "includeAllUrlParameters": true 
            }] 
        } 
    }] 
}
```

## 錯誤回應

傳遞至 error 回呼的 &quot;status&quot; 和 &quot;error&quot; 參數將具備下列格式:

| 名稱 | 類型 | 說明 |
|--- |--- |--- |
| 狀態 | 字串 | 代表錯誤狀態。 此參數可具備下列值:<ul><li>timeout：表示要求逾時。</li><li>parseerror: 指出無法剖析回應，例如收到 HTML 或純文字而非 JSON。</li><li>error: 指出一般錯誤，例如收到 200 OK 以外的其他 HTTP 狀態</li></ul> |
| error | 字串 | 包含其他資料 (例如例外訊息) 或對於疑難排解可能實用的任何項目。 |
