---
keywords: at.js版本、at.js版本、單頁應用程式、spa、跨網域、跨網域
description: 瞭解如何從 [!DNL Adobe Target] at.js 1.x升級為at.js 2.x。 檢查系統流程圖表、瞭解新功能和已棄用的功能等等。
title: 如何從at.js 1.x版升級至2.x版？
feature: at.js
exl-id: fbfa5743-0fa5-44c6-89b3-fdee9b50e126
TQID: https://experienceleague.adobe.com/JGsuogzhCvThr4QGHJ5g4d8ZqGVZ8ClAf7hXuoh7X0Q
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: df62f171-ac37-440f-8f0f-f41a72ebdd34
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 929e1f10bc5dd0741f0fe28cd46435e680a4a308
workflow-type: tm+mt
source-wordcount: 3100
ht-degree: 47%

---

# 從at.js 1.*x*&#x200B;升級為at.js 2.*x*

[!DNL Adobe Target] 中的最新版 at.js 提供豐富的功能，讓貴公司能以新世代用戶端技術為基礎進行個人化。 本次的新版本著重於升級 at.js，進而與單一頁面應用程式 (SPA) 產生和諧互動。

以下是幾個使用at.js 2.*x*&#x200B;特有（舊版未提供）的優點：

* 可在頁面載入時快取所有產品建議，以減少對單一伺服器呼叫發出的多個伺服器呼叫。
* 大幅改善一般使用者在網站上的體驗，因為產品建議能透過快取立即顯示，避免傳統伺服器呼叫引發的延遲時間。
* 簡單的單行程式碼和一次性開發人員設定，讓行銷人員可透過VEC在SPA上建立和執行[!UICONTROL A/B測試]和[!UICONTROL 體驗鎖定目標] (XT)活動。

## at.js 2.*x*&#x200B;系統圖表

下列圖表可協助您瞭解at.js 2.*x*&#x200B;搭配檢視的工作流程，以及如何藉由這套工作流程增強SPA整合。 若要更詳細地瞭解at.js 2.*x*&#x200B;中使用的概念，請參閱[單頁應用程式實作](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。

（按一下影像可展開至完整寬度。）

![使用at.js 2.x](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "使用at.js 2.x"){zoomable="yes"}的Target流程

| 呼叫 | 詳細資料 |
| --- | --- |
| 1 | 如果使用者已通過驗證，呼叫會傳回[!UICONTROL Experience Cloud ID]；另一個呼叫會同步客戶ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<P>也能使用將頁面上實作的程式碼片段預先隱藏的選項，以非同步方式載入at.js。 |
| 3 | 提出頁面載入要求，包含所有已設定的參數 (MCID、SDID 和客戶 ID)。 |
| 4 | 個人資料指令碼執行，然後注入個人資料存放區。 存放區會從對象資料庫中要求合格對象（例如從[!DNL Adobe Analytics]、[!DNL Audience Manager]等共用的對象）。<P>客戶屬性會透過批次程序傳送至設定檔存放區。 |
| 5 | [!DNL Target] 會根據 URL 要求參數和個人資料，決定可針對目前頁面和未來檢視傳回哪些活動和體驗給訪客。 |
| 6 | 目標內容會傳回至頁面，選擇性地包括其他個人化的個人資料值。<P>目前頁面上目標內容會儘快出現，不會有忽隱忽現的預設內容。<P>在瀏覽器中快取的使用者SPA動作顯示檢視的目標內容，以便在透過`triggerView()`觸發檢視時立刻套用，不需額外的伺服器呼叫。 |
| 7 | [!UICONTROL Analytics] 資料傳送至「資料收集」伺服器。 |
| 8 | 目標資料透過SDID與[!UICONTROL Analytics]資料相符，並處理至[!UICONTROL Analytics]報表儲存體。<P>然後就可以透過[!UICONTROL Analytics for Target] (A4T)報表，在[!UICONTROL Analytics]和[!DNL Target]中檢視[!UICONTROL Analytics]資料。 |

現在，SPA 上只要是有實作 `triggerView()` 的位置，系統都會從快取擷取檢視和動作並向使用者顯示，不需要伺服器呼叫。 `triggerView()` 也會對 [!DNL Target] 後端發出通知要求，以便增加和記錄曝光計數。

（按一下影像可展開至完整寬度。）

![Target流程at.js 2.*x* triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Target流程at.js 2.*x* triggerView"){zoomable="yes"}

| 呼叫 | 詳細資料 |
| --- | --- |
| 1 | 系統在 SPA 中呼叫 `triggerView()`，以便呈現檢視和套用動作來修改視覺元素。 |
| 2 | 從快取讀取檢視的目標內容。 |
| 3 | 目標內容會儘快出現，不會有忽隱忽現的預設內容。 |
| 4 | 通知要求會傳送至 [!DNL Target] 個人資料存放區，以計算活動中的訪客數和增加量度。 |
| 5 | [!UICONTROL Analytics]資料已傳送至資料收集伺服器。 |
| 6 | [!DNL Target]資料透過SDID與[!UICONTROL Analytics]資料相符，並處理至[!UICONTROL Analytics]報表儲存體。 然後就可以在[!UICONTROL Analytics]與[!DNL Target]中透過A4T報表檢視[!UICONTROL Analytics]資料。 |

## 部署at.js 2.*x*

透過[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)擴充功能中的標籤部署at.js 2.*x*。

>[!NOTE]
>
>使用[!DNL Adobe Experience Platform]中的標籤部署at.js是偏好使用的方法。
>
>或
>
>使用[!DNL Target] UI手動下載at.js 2.*x*，並使用您選擇的[方法](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md)進行部署。

## 棄用的 at.js 函數

at.js 2.*x*&#x200B;已棄用多個函式。

>[!WARNING]
>
>如果部署at.js 2.*x*&#x200B;時您的網站上仍使用這些已棄用的函式，便會顯示主控台警告。 建議的升級做法是在預備環境中測試at.js 2.*x*&#x200B;的部署，並確實逐一瀏覽每個記錄到主控台中的警告，並將棄用的函式轉譯為at.js 2.*x*&#x200B;中推出的新函式。

已棄時的函數及其對應的新函數如下所列。 如需完整的函數清單，請參閱 [at.js 函數](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md)。

>[!NOTE]
>
>at.js 2.*x*&#x200B;不再自動預先隱藏標示為`mboxDefault`的元素。 因此客戶必須在網站上或透過標籤管理程式，手動提供預先隱藏邏輯。

### mboxCreate(mbox,params)

**說明**:

執行要求並將產品建議套用至具有 `mboxDefault` 類別名稱的最接近 DIV。

**範例**:

```html {line-numbers="true"}
<div class="mboxDefault">
  default content to replace by offer
</div>
<script>
  mboxCreate('mboxName','param1=value1','param2=value2');
</script>
```

**at.js 2.*x*對等專案**

`mboxCreate(mbox, params)` 的替代函數為 `getOffer()` 和 `applyOffer()`。

**範例**:

```html {line-numbers="true"}
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  var el = document.currentScript.previousElementSibling;
  adobe.target.getOffer({
    mbox: "mboxName",
    params: {
      param1: "value1",
      param2: "value2"
    },
    success: function(offer) {
      adobe.target.applyOffer({
        mbox: "mboxName",
        selector: el,
        offer: offer
      });
    },
    error: function(error) {
      console.error(error);
      el.style.visibility = "visible";
    }
  });
</script> 
```

### mboxDefine() 和 mboxUpdate()

**說明**:

在元素與 mbox 名稱之間建立內部對應，但是不執行要求。 與 `mboxUpdate()` 搭配使用，會執行要求並將產品建議套用至 `mboxDefine()` 中的 nodeId 所識別的元素。 也可用來更新 `mboxCreate` 起始的 mbox。

**範例**:

```html {line-numbers="true"}
<div id="someId" class="mboxDefault"></div>
<script>
 mboxDefine('someId','mboxName','param1=value1','param2=value2');
 mboxUpdate('mboxName','param3=value3','param4=value4');
</script>
```

**at.js 2.*x*對等專案**：

`mboxDefine()` 和 `mboxUpdate` 的替代函數為 `getOffer()` 和 `applyOffer()`，而 `applyOffer()` 中會使用選取器選項。 此做法可讓您使用任何 CSS 選取器將產品建議對應至元素，而非只能使用帶有 ID 的選取器。

**範例**:

```html {line-numbers="true"}
<div id="someId" class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  adobe.target.getOffer({
    mbox: "mboxName",
    params: {
      param1: "value1",
      param2: "value2",
      param3: "value3",
      param4: "value4" 
    },
    success: function(offer) {
      adobe.target.applyOffer({
        mbox: "mboxName",
        selector: "#someId",
        offer: offer
      });
    },
    error: function(error) {
      console.error(error);
      var el = document.getElementById("someId");
      el.style.visibility = "visible";
    }
  });
</script>
```

### adobe.target.registerExtension()

**說明**:

提供標準方式來註冊特定的延伸模組。

此函數不再受支援且不應使用。

## 2.*x*&#x200B;中已棄用、新增和支援的at.js函式摘要

| 方法 | 支援? | 新增? | 已過時?<P>（將顯示預設內容） |
| --- | --- | --- | --- |
| `getOffer()` | 是 |  |  |
| `getOffers()` |  | 是 |  |
| `applyOffer()` | 是 |  |  |
| `applyOffers()` |  | 是 |  |
| `triggerView()` |  | 是 |  |
| `trackEvent()` | 是 |  |  |
| `mboxCreate()` |  |  | 是 |
| `mboxDefine()`<P>`mboxUpdate()` |  |  | 是 |
| `targetGlobalSettings()` | 是 |  |  |
| `Data Providers` | 是 |  |  |
| `targetPageParams()` | 是 |  |  |
| `targetPageParamsAll()` | 是 |  |  |
| `registerExtension()` |  |  | 是 |
| `At.js Custom Events` | 是 |  |  |

## 限制和圖說

請留意下列限制和圖說:

### 轉換追蹤

客戶使用 `mboxCreate()` 追蹤轉換時必須使用 `trackEvent()` 或 `getOffer()`。

### 產品建議傳送

客戶若沒有將 `mboxCreate()` 取代為 `getOffer()` 或 `applyOffer()`，產品建議可能不會傳送。

### 若某些頁面上具有at.js 1.*x*，那麼可以在其他頁面上使用at.js 2.*x*&#x200B;嗎？

可以，使用不同版本和資料庫的頁面中會保留訪客輪廓。 Cookie 格式相同。

### at.js 2.*x*&#x200B;使用的新API

at.js 2.*x*&#x200B;使用新的API，我們稱之為「傳送API」。 若要針對at.js是否正確呼叫[!DNL Target] Edge伺服器進行除錯，您可以將瀏覽器上「開發人員工具」的「網路」索引標籤篩選為「傳送」、「`tt.omtrdc.net`」或您的使用者端代碼。 您也會發現 [!DNL Target] 傳送的是 JSON 裝載而非機碼值組。

### [!DNL Target]全域mbox已不再使用

在at.js 2.*x*&#x200B;中，網路呼叫中不會再出現「`target-global-mbox`」。 我們已改為將傳送至[!DNL Target]伺服器的JSON裝載中的&quot;`target-global-mbox`&quot;語法取代為&quot;`execute > pageLoad`&quot;，如下所示：

```json {line-numbers="true"}
{
  "id": {
    // ...
  },
  "context": {
    "channel": "web",
    // ...
  },
  "execute": {
    "pageLoad": {}
  }
}
```

基本上，推出全域 mbox 概念的目的，是用來告知 [!DNL Target] 是否要在頁面載入時擷取產品建議和內容。 因此我們在最新版本中讓這個用途更加明確。

### at.js 中的全域 mbox 名稱仍重要嗎?

客戶可以透過&#x200B;**[!UICONTROL Target]** > **[!UICONTROL 管理]** > **[!UICONTROL 實作]** > **[!UICONTROL 編輯at.js設定]**&#x200B;來指定全域mbox名稱。 此設定供 [!DNL Target] Edge 伺服器用來將 execute > pageLoad 轉譯為 [!DNL Target] UI 中顯示的全域 mbox 名稱。 這麼做可讓客戶繼續使用伺服器端 API、表單式撰寫器、輪廓指令碼，以及使用全域 mbox 名稱建立客群。 強烈建議您也務必在&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 視覺化體驗撰寫器]**&#x200B;頁面上設定相同的全域mbox名稱，以防仍有頁面使用at.js 1.*x*，如下列圖例所示。

![修改 at.js 對話方塊](../assets/modify-atjs.png)

和

![自訂全域 mbox](../assets/custom-global-mbox.png)

### 需要為at.js 2.*x*&#x200B;開啟自動建立全域mbox設定嗎？

在大多數情況下需要。 此設定可告知at.js 2.*x*&#x200B;在頁面載入時向[!DNL Target]邊緣伺服器觸發要求。 由於全域 mbox 已轉譯為 execute > pageLoad，因此如果您想在頁面載入時觸發要求，便應開啟此設定。

### 即使沒有從at.js 2.*x*&#x200B;指定Target全域mbox名稱，現有的VEC活動仍會繼續運作嗎？

會，因為 execute > pageLoad 在 [!DNL Target] 後端上的處理方式如同 `target-global-mbox`。

### 如果我的表單式活動目標鎖定為 `target-global-mbox`，這些活動是否仍會繼續運作?

會，因為 execute > pageLoad 在 [!DNL Target] Edge 伺服器上的處理方式如同 `target-global-mbox`。

### 支援和不支援的at.js 2.*x*&#x200B;設定

| 設定 | 支援? |
| --- | --- |
| X-Domain | 無 |
| 自動建立全域 mbox | 是 |
| 全域 mbox 名稱 | 是 |

### at.js 2.x中的跨網域追蹤支援

跨網域追蹤能夠拼接不同網域之間的訪客。 由於必須為各個網域建立新 Cookie，因此在訪客從某個網域導覽至另一個網域時，就很難加以追蹤。 為實現跨網域追蹤，[!DNL Target] 使用第三方 Cookie 來追蹤網域之間的訪客。 這可讓您建立橫跨`siteA.com`和`siteB.com`的[!DNL Target]活動，而且當訪客在各個唯一網域導覽時，訪客會維持相同的體驗。 此功能與[!DNL Target]的第三方和第一方Cookie行為整合。

>[!NOTE]
>
>自at.js 2.10起支援跨網域追蹤，但在2.10之前的at.js 2.*x*&#x200B;中不支援立即使用。 at.js 2.*x*&#x200B;可透過Experience Cloud ID (ECID)資料庫v4.3.0+支援跨網域追蹤。

在[!DNL Target]中，協力廠商Cookie儲存在`<CLIENTCODE>.tt.omtrdc.net`中。 第一方 Cookie 會儲存在 `clientdomain.com` 中。 第一個要求會傳回 HTTP 回應標頭，此標頭嘗試設定名為 `mboxSession` 和 `mboxPC` 的第三方 Cookie，之後傳回附有額外參數 (`mboxXDomainCheck=true`) 的重新導向要求。 若瀏覽器接受第三方 Cookie，則重新導向請求會包含這些 Cookie 並傳回體驗。 這個工作流程之所以可行，是因為我們使用 HTTP GET 方法。

不過，在at.js 2.*x*&#x200B;中，並未使用HTTP GET。 而是透過at.js 2.*x*&#x200B;使用HTTP POST將JSON裝載傳送至[!DNL Target]部Edge伺服器。 使用HTTP POST表示檢查瀏覽器是否支援第三方Cookie的重新導向要求將中斷。 這是因為 HTTP GET 要求為等冪交易，而 HTTP POST 是非等冪交易且不得任意重複。 因此，不支援立即使用at.js 2.*x* （2.10之前）進行跨網域追蹤。 只有at.js 1.*x*&#x200B;才有跨網域追蹤的立即可用支援。

若要針對at.js v2.10或更新版本使用跨網域追蹤，您可以執行下列其中一項作業：

1. 安裝[ECID程式庫v4.3.0+](https://experienceleague.adobe.com/docs/id-service/using/release-notes/release-notes.html??lang=zh-Hant)和at.js 2.*x*。 ECID 資料庫的存在是為了管理可用來識別訪客 (甚至是跨網域) 的持續 ID。 安裝ECID資料庫v4.3.0+和at.js 2.*x*&#x200B;後，您將能夠建立橫跨多個唯一網域的活動以及追蹤使用者。 請務必注意，此功能僅適用於工作階段過期之後。

1. 如果您有at.js v2.10或更新版本，您不必安裝ECID程式庫，而是可以在&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 實作]**&#x200B;的[!DNL Target] UI中啟用跨網域設定。 （或者，您可以在at.js程式碼中將&#x200B;_crossDomain_&#x200B;選項設為&#x200B;_enabled_。）

若要對2.10之前的at.js v2.*x*&#x200B;版本使用跨網域追蹤，您可以實作上述選項#1 （安裝ECID程式庫）。

### 支援自動建立全域 mbox

此設定可告知at.js 2.*x*&#x200B;在頁面載入時向[!DNL Target]邊緣伺服器觸發要求。 由於全域 mbox 已轉譯為 execute > pageLoad，且會由 [!DNL Target] Edge 伺服器加以解譯，因此顧客如果希望在頁面載入時觸發要求，便應開啟此功能。

### 支援全域 mbox 名稱

客戶可以透過&#x200B;**[!UICONTROL Target]** > **[!UICONTROL 管理]** > **[!UICONTROL 實作]** > **[!UICONTROL 編輯]**&#x200B;指定全域mbox名稱。 此設定供 [!DNL Target] Edge 伺服器用來將 execute > pageLoad 轉譯為輸入的全域 mbox 名稱。 這麼做可讓客戶繼續使用伺服器端 API、表單式撰寫器、輪廓指令碼，以及建立鎖定全域 mbox 為目標的客群。

### 以下 at.js 自訂事件適用於 `triggerView()`，還是僅適用於 `applyOffer()` 或 `applyOffers()`?

* `adobe.target.event.CONTENT_RENDERING_FAILED`
* `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`
* `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`
* `adobe.target.event.CONTENT_RENDERING_REDIRECT`

是的，at.js 自訂事件也適用於 `triggerView()`。

### 它表示當我使用&amp;lbrace；`"page" : "true"`&amp;rbrace；呼叫`triggerView()`時，它會傳送通知至[!DNL Target]後端並增加曝光次數。 那麼也會造成輪廓指令碼執行嗎?

對 [!DNL Target] 後端發出預先擷取呼叫時，會執行輪廓指令碼， 接著會加密受影響的輪廓資料，然後傳回用戶端。 叫用含有 `{"page": "true"}` 的 `triggerView()` 後，會傳送通知並附上加密的輪廓資料。 此時 [!DNL Target] 後端會將輪廓資料解密並儲存至資料庫中。

### 呼叫 `triggerView()` 之前需要新增預先隱藏程式碼以處理忽隱忽現情況嗎?

不需要，您不需要在呼叫 `triggerView()` 之前新增預先隱藏程式碼。 at.js 2.*x*&#x200B;會在顯示和套用檢視之前，處理預先隱藏和忽隱忽現邏輯。

### at.js 2.*x*&#x200B;不支援建立對象的at.js 1.*x*&#x200B;引數？

使用at.js 2.*x*&#x200B;時，下列at.js 1.x引數目前不支援對象建立&#x200B;*NOT*：

* browserHeight
* browserWidth
* browserTimeOffset
* screenHeight
* screenWidth
* screenOrientation
* colorDepth
* devicePixelRatio
* vst.*引數（請參閱下文）

### at.js 2.*x*&#x200B;不支援使用vst.*引數建立對象

at.js 1.*x*&#x200B;的客戶可以使用vst.* mbox引數建立對象。 這是at.js 1.*x*&#x200B;傳送mbox引數至[!DNL Target]後端時的意外副作用。 移轉至at.js 2.*x*&#x200B;後，您無法再使用這些引數建立對象，因為at.js 2.*x*&#x200B;傳送mbox引數的方式不同。

## at.js 相容性

下表說明 at.js。 2.*x*&#x200B;與不同活動型別、整合、功能和at.js函式的相容性。

### 活動類型

| 類型 | 支援? |
| --- | --- |
| [!UICONTROL A/B 測試] | 是 |
| [!UICONTROL 自動分配] | 是 |
| [!UICONTROL 自動鎖定目標] | 是 |
| [!UICONTROL 體驗鎖定目標] | 是 |
| [!UICONTROL 多變數測試] | 是 |
| [!UICONTROL 自動個人化] | 是 |
| [!DNL Recommendations] | 是 |

>[!NOTE]
>
>當所有修改皆已套用至`Page Load Event`時，可透過at.js 2.*x*&#x200B;和VEC支援[!UICONTROL 自動鎖定目標]活動。 將修改新增至特定檢視時，僅支援[!UICONTROL A/B測試]、[!UICONTROL 自動分配]和[!UICONTROL 體驗鎖定目標] (XT)活動。

### 整合

| 類型 | 支援? |
| --- | --- |
| [!UICONTROL Analytics for Target] (A4T) | 是 |
| 客群 | 是 |
| 客戶屬性 | 是 |
| AEM 體驗片段 | 是 |
| [Adobe Experience Platform擴充功能](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) | 是 |
| 除錯程式 | 是 |
| Auditor | 尚未針對at.js 2.*x*&#x200B;更新規則 |
| [GDPR](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)的選擇加入支援 | [at.js 2.1.0](/help/dev/implement/client-side/atjs/target-atjs-versions.md#atjs-version-210-june-3-2019)版或更新版本支援此功能。 |
| 由[!DNL Adobe Target]提供支援的AEM Enhanced Personalization | 無 |

### 功能

| 功能 | 支援? |
| --- | --- |
| X-Domain | 無 |
| 屬性/工作區 | 是 |
| QA 連結 | 是 |
| 表單式體驗撰寫器 | 是 |
| 可視化體驗撰寫器 (VEC) | 是 |
| 自訂程式碼 | 是 |
| [回應權杖](#response-tokens) | 是 |
| 點擊追蹤 | 是 |
| 多活動傳送 | 是 |
| targetGlobalSettings | 支援 (但不支援跨網域) |
| at.js 方法 | 除了以下專案以外均支援：<P>`mboxCreate()`<P>`mboxUpdate()`<P>`mboxDefine()`<P>將顯示預設內容。 |

### 查詢字串參數

| 參數 | 支援? |
| --- | --- |
| `?mboxDisable` | 是 |
| `?mboxDisable` | 是 |
| `?mboxTrace` | 是 |
| `?mboxSession` | 無 |
| `?mboxOverride.browserIp` | 是 |

## 回應 Token

at.js 2.*x*，就像at.js 1.*x*&#x200B;一樣，會使用自訂事件`at-request-succeeded`來顯示回應Token。 如需使用 `at-request-succeeded` 自訂事件的程式碼範例，請參閱[回應 Token](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)。

## at.js 1.*x*&#x200B;引數至at.js 2.*x*&#x200B;裝載對應

本節概述at.js 1.*x*&#x200B;與at.js 2.*x*&#x200B;之間的對應。

探索參數對應之前，請注意這些資料庫版本使用的端點已變更:

* at.js 1.*x* - `http://<client code>.tt.omtrdc.net/m2/<client code>/mbox/json`
* at.js 2.*x* - `http://<client code>.tt.omtrdc.net/rest/v1/delivery`

另一項顯著差異為:

* at.js 1.*x* — 使用者端代碼是路徑的一部分
* at.js 2.*x* — 使用者端代碼會以查詢字串引數的形式傳送，例如：
  `http://<client code>.tt.omtrdc.net/rest/v1/delivery?client=democlient`

以下區段列出每個at.js 1.*x*&#x200B;引數、其說明，以及相對應的2.*x* JSON裝載（若適用）：

### at_property

（at.js 1.*x*&#x200B;引數）

用於[企業使用者權限](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html??lang=zh-Hant)。

```json {line-numbers="true"}
{
  ....
  "property": {
    "token": "1213213123122313121"
  }
  ....
}
```

### mboxHost

（at.js 1.*x*&#x200B;引數）

[!DNL Target]資料庫執行所在頁面的網域。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "context": {
    "browser": {
       "host": "test.com"
    }
  }
}
```

### webGLRenderer

（at.js 1.*x*&#x200B;引數）

瀏覽器的 WEB GL 轉譯器功能 我們的裝置偵測機制會使用此功能，判斷訪客的裝置是否為桌上型電腦、iPhone、Android 裝置等。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "context": {
    "browser": {
       "webGLRenderer": "AMD Radeon Pro 560X OpenGL Engine"
    }
  }
}
```

### mboxURL

（at.js 1.*x*&#x200B;引數）

頁面 URL。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "context": {
    "address": {
       "url": "http://test.com"
    }
  }
}
```

### mboxReferrer

（at.js 1.*x*&#x200B;引數）

頁面轉介者。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "context": {
    "address": {
       "referringUrl": "http://google.com"
    }
  }
}
```

### mbox (名稱) 等於全域 mbox

（at.js 1.*x*&#x200B;引數）

傳遞 API 已無全域 mbox 概念。 在 JSON 裝載中，您必須使用 `execute > pageLoad`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "execute": {
    "pageLoad": {
       "parameters": ....
       "profileParameters": ...
       .....
    }
  }
}
```

### mbox (名稱) *不*&#x200B;等於全域 mbox

（at.js 1.*x*&#x200B;引數）

若要使用 mbox 名稱，請將其傳遞至 `execute > mboxes`。 mbox 需要索引和名稱。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "execute": {
    "mboxes": [{
       "index": 0,
       "name": "some-mbox",
       "parameters": ....
       "profileParameters": ...
       .....
    }]
  }
}
```

### mboxId

（at.js 1.*x*&#x200B;引數）

已不再使用。

### mboxCount

（at.js 1.*x*&#x200B;引數）

已不再使用。

### mboxRid

（at.js 1.*x*&#x200B;引數）

下游系統用來協助進行偵錯的要求 ID。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "requestId": "2412234442342"
  ....
}
```

### mboxTime

（at.js 1.*x*&#x200B;引數）

已不再使用。

### mboxSession

（at.js 1.*x*&#x200B;引數）

工作階段 ID 會以查詢字串參數 (`sessionId`) 的形式傳送至傳送 API 端點。

### mboxPC

（at.js 1.*x*&#x200B;引數）

TNT ID 會傳遞至 `id > tntId`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "id": {
    "tntId": "ca5ddd7e33504c58b70d45d0368bcc70.21_3"
  }
  ....
}
```

### mboxMCGVID

（at.js 1.*x*&#x200B;引數）

Marketing Cloud 訪客 ID 會傳遞至 `id > marketingCloudVisitorId`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "id": {
    "marketingCloudVisitorId": "797110122341429343505"
  }
  ....
}
```

### `vst.aaaa.id`和`vst.aaaa.authState`

（at.js 1.*x*&#x200B;引數）

客戶 ID 應傳遞至 `id > customerIds`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "id": {
    "customerIds": [{
       "id": "1232131",
       "integrationCode": "aaaa",
       "authenticatedState": "....."
     }]
  }
  ....
}
```

### mbox3rdPartyId

（at.js 1.*x*&#x200B;引數）

用來連結不同[!DNL Target] ID的客戶協力廠商ID。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "id": {
    "thirdPartyId": "1232312323123"
  }
  ....
}
```

### mboxMCSDID

（at.js 1.*x*&#x200B;引數）

SDID (也稱為補充資料 ID)。 應傳遞至 `experienceCloud > analytics > supplementalDataId`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "supplementalDataId": "1212321132123131"
    }
  }
  ....
}
```

### vst.trk

（at.js 1.*x*&#x200B;引數）

[!UICONTROL Analytics]追蹤伺服器 應傳遞至 `experienceCloud > analytics > trackingServer`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "trackingServer": "analytics.test.com"
    }
  }
  ....
}
```

### vst.trks

（at.js 1.*x*&#x200B;引數）

Analytics 追蹤伺服器安全。 應傳遞至 `experienceCloud > analytics > trackingServerSecure`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "experienceCloud": {
    "analytics": {
      "trackingServerSecure": "secure-analytics.test.com"
    }
  }
  ....
}
```

### mboxMCGLH

（at.js 1.*x*&#x200B;引數）

Audience Manager 位置提示。 應傳遞至 `experienceCloud > audienceManager > locationHint`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "experienceCloud": {
    "audienceManager": {
      "locationHint": 9
    }
  }
  ....
}
```

### mboxAAMB

（at.js 1.*x*&#x200B;引數）

Audience Manager Blob。 應傳遞至 `experienceCloud > audienceManager > blob`。

at.js 2.*x* JSON裝載：

```json {line-numbers="true"}
{
  "experienceCloud": {
    "audienceManager": {
      "blob": "2142342343242342"
    }
  }
  ....
}
```

### mboxVersion

（at.js 1.*x*&#x200B;引數）

版本會透過版本參數以查詢字串參數的形式傳送。

## 訓練影片： at.js 2.*x*&#x200B;架構圖表![總覽徽章](../../assets/overview.png)

at.js 2.*x*&#x200B;增強了Adobe [!DNL Target]對SPA的支援，並與其他Experience Cloud解決方案整合。 本影片說明整合方式。

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

如需詳細資訊，請參閱[瞭解at.js 2.*x*&#x200B;的運作方式](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html)。

