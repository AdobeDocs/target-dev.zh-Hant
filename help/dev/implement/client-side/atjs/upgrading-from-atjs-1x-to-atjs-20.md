---
keywords: at.js版本、at.js版本、單頁應用程式、spa、跨網域、跨網域
description: 瞭解如何從升級 [!DNL Adobe Target] at.js 1.x至at.js 2.x。檢查系統流程圖表、瞭解新功能和已棄用的功能等等。
title: 如何從at.js 1.x版升級至2.x版？
feature: at.js
exl-id: fbfa5743-0fa5-44c6-89b3-fdee9b50e126
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '2925'
ht-degree: 73%

---

# 從 at.js 1.*x* 升級為 at.js 2.*x*

[!DNL Adobe Target] 中的最新版 at.js 提供豐富的功能，讓貴公司能以新世代用戶端技術為基礎進行個人化。本次的新版本著重於升級 at.js，進而與單一頁面應用程式 (SPA) 產生和諧互動。

以下是幾個使用 at.js 2.*x* 特有 (舊版未提供) 的優點:

* 可在頁面載入時快取所有選件，以減少對單一伺服器呼叫發出的多個伺服器呼叫。
* 大幅改善一般使用者在網站上的體驗，因為選件能透過快取立即顯示，避免傳統伺服器呼叫引發的延遲時間。
* 簡單的單行程式碼和一次性開發人員設定，讓行銷人員可建立和執行 [!UICONTROL A/B測試] 和 [!UICONTROL 體驗鎖定] (XT)活動(透過SPA上的VEC)。

## at.js 2.*x* 系統圖表

下列圖表可協助您瞭解 at.js 2.*x* 搭配檢視的工作流程，以及如何藉由這套工作流程增強 SPA 整合。如需 at.js 2.*x* 中所使用概念的詳細介紹，請參閱[實作單頁應用程式](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)。

（按一下影像可展開至完整寬度。）

![使用at.js 2.x的Target流程](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "使用at.js 2.x的Target流程"){zoomable=&quot;yes&quot;}

| 呼叫 | 詳細資料 |
| --- | --- |
| 1 | 呼叫會傳回 [!UICONTROL EXPERIENCE CLOUDID] 如果使用者已通過驗證；另一個呼叫會同步客戶ID。 |
| 2 | at.js 程式庫會同步載入並隱藏文件本文。<P>也能使用將頁面上實作的程式碼片段預先隱藏的選項，以非同步方式載入 at.js。 |
| 3 | 提出頁面載入要求，包含所有已設定的參數 (MCID、SDID 和客戶 ID)。 |
| 4 | 個人資料指令碼執行，然後注入個人資料存放區。存放區會從受眾資料庫中要求合格受眾 (例如從 [!DNL Adobe Analytics]、[!DNL Audience Manager] 等共用的受眾)。<P>客戶屬性會透過批次程序傳送至個人資料存放區。 |
| 5 | [!DNL Target] 會根據 URL 要求參數和個人資料，決定可針對目前頁面和未來檢視傳回哪些活動和體驗給訪客。 |
| 6 | 目標內容會傳回至頁面，選擇性地包括其他個人化的個人資料值。<P>目前頁面上目標內容會儘快出現，不會有忽隱忽現的預設內容。<P>針對在瀏覽器中快取的使用者 SPA 動作顯示檢視的目標內容，以便在透過 `triggerView()` 觸發檢視時立刻套用，不需額外的伺服器呼叫。 |
| 7 | [!UICONTROL Analytics 資料傳送至「資料收集」伺服器。] |
| 8 | 目標資料符合 [!UICONTROL Analytics] 資料會透過SDID傳遞並處理至 [!UICONTROL Analytics] 報告儲存體。<P>然後就可以在 [!UICONTROL Analytics] 與 [!UICONTROL  中，透過 ]Analytics for Target[!DNL Target] (A4T) 報表來檢視 [!UICONTROL Analytics] 資料。 |

現在，SPA 上只要是有實作 `triggerView()` 的位置，系統都會從快取擷取檢視和動作並向使用者顯示，不需要伺服器呼叫。`triggerView()` 也會對 [!DNL Target] 後端發出通知要求，以便增加和記錄曝光計數。

（按一下影像可展開至完整寬度。）

![Target流程at.js 2.*x* triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Target流程at.js 2.*x* triggerView"){zoomable=&quot;yes&quot;}

| 呼叫 | 詳細資料 |
| --- | --- |
| 1 | 系統在 SPA 中呼叫 `triggerView()`，以便呈現檢視和套用動作來修改視覺元素。 |
| 2 | 從快取讀取檢視的目標內容。 |
| 3 | 目標內容會儘快出現，不會有忽隱忽現的預設內容。 |
| 4 | 通知要求會傳送至 [!DNL Target] 個人資料存放區，以計算活動中的訪客數和增加量度。 |
| 5 | [!UICONTROL Analytics 資料傳送至資料收集伺服器。] |
| 6 | [!DNL Target] 資料會透過 SDID 來比對 Analytics 資料，然後經過處理放入 Analytics 報表儲存體中。[!UICONTROL 然後就可以在 Analytics 與 中，透過 A4T 報表來檢視 Analytics 資料。][!DNL Target] |

## 部署 at.js 2.*x*

部署 at.js 2.*x* 透過中的標籤 [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 副檔名。

>[!NOTE]
>
>使用中的標籤部署at.js [!DNL Adobe Experience Platform] 是偏好方法。
>
>或
>
>手動下載at.js 2.*x* 使用 [!DNL Target] UI並使用 [您選擇的方法](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md).

## 棄用的 at.js 函數

at.js 2.*x* 已棄用多個函數。

>[!WARNING]
>
>如果部署 at.js 2.*x* 時您的網站上仍使用這些已棄用的函數，便會顯示主控台警告。建議的升級做法是在預備環境中測試 at.js 2.*x* 部署，並確實逐一瀏覽每個記錄到主控台中的警告，並將棄用的函數轉譯為 at.js 2.*x* 中推出的新函數。

已棄時的函數及其對應的新函數如下所列。如需完整的函數清單，請參閱 [at.js 函數](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md)。

>[!NOTE]
>
>at.js 2.*x* 不再自動預先隱藏標示為 `mboxDefault` 的元素。因此客戶必須在網站上或透過標籤管理程式，手動提供預先隱藏邏輯。

### mboxCreate(mbox,params)

**說明**:

執行要求並將選件套用至具有 `mboxDefault` 類別名稱的最接近 DIV。

**範例**:

```html {line-numbers="true"}
<div class="mboxDefault">
  default content to replace by offer
</div>
<script>
  mboxCreate('mboxName','param1=value1','param2=value2');
</script>
```

**at.js 2.*x* 等同項目**

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

在元素與 mbox 名稱之間建立內部對應，但是不執行要求。與 `mboxUpdate()` 搭配使用，會執行要求並將選件套用至 `mboxDefine()` 中的 nodeId 所識別的元素。也可用來更新 `mboxCreate` 起始的 mbox。

**範例**:

```html {line-numbers="true"}
<div id="someId" class="mboxDefault"></div>
<script>
 mboxDefine('someId','mboxName','param1=value1','param2=value2');
 mboxUpdate('mboxName','param3=value3','param4=value4');
</script>
```

**at.js 2.*x* 等同項目**:

`mboxDefine()` 和 `mboxUpdate` 的替代函數為 `getOffer()` 和 `applyOffer()`，而 `applyOffer()` 中會使用選取器選項。此做法可讓您使用任何 CSS 選取器將選件對應至元素，而非只能使用帶有 ID 的選取器。

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

## 2 中棄用、新增和支援的 at.js 函數摘要.*x*

| 方法 | 支援? | 新增? | 已過時? <P> (將顯示預設內容) |
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

### 選件傳送

客戶若沒有將 `mboxCreate()` 取代為 `getOffer()` 或 `applyOffer()`，選件可能不會傳送。

### at.js 2.*x* 是否能在某些頁面上使用，並在其他頁面上使用 at.js 1.*x* 是否在其他頁面上？

可以，使用不同版本和資料庫的頁面中會保留訪客設定檔。Cookie 格式相同。

### at.js 2.*x* 使用的新 API

at.js 2.*x* 使用新的 API，我們稱之為「傳送 API」。為了偵錯at.js是否正在呼叫 [!DNL Target] Edge Server正確無誤，您可將瀏覽器上「開發人員工具」的「網路」索引標籤篩選為「傳送」，「`tt.omtrdc.net`，」或您的使用者端代碼。 您也會發現 [!DNL Target] 傳送的是 JSON 裝載而非機碼值組。

### [!DNL Target]不再使用 全域 mbox

在 at.js 2.*x*，您不會再看到「`target-global-mbox`」會顯示在網路呼叫中。 我們已取代「`target-global-mbox`「語法至」`execute > pageLoad`」在傳送至的JSON裝載中 [!DNL Target] 伺服器，如下所示：

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

基本上，推出全域 mbox 概念的目的，是用來告知 [!DNL Target] 是否要在頁面載入時擷取選件和內容。因此我們在最新版本中讓這個用途更加明確。

### at.js 中的全域 mbox 名稱仍重要嗎?

客戶可透過以下方式指定全域mbox名稱： **[!UICONTROL Target]** > **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 編輯at.js設定]**. 此設定供 [!DNL Target] Edge 伺服器用來將 execute > pageLoad 轉譯為 [!DNL Target] UI 中顯示的全域 mbox 名稱。這麼做可讓客戶繼續使用伺服器端 API、表單式撰寫器、設定檔指令碼，以及使用全域 mbox 名稱建立對象。我們強烈建議您也務必確認上已設定相同的全域mbox名稱， **[!UICONTROL 管理]** > **[!UICONTROL 視覺化體驗撰寫器]** 頁面，以防仍有頁面使用at.js 1.*x*，如下圖所示。

![修改 at.js 對話方塊](../assets/modify-atjs.png)

和

![自訂全域 mbox](../assets/custom-global-mbox.png)

### 需要為 at.js 2.*x* 開啟自動建立全域 mbox 設定嗎?

在大多數情況下需要。此設定可告知 at.js 2.*x* 在頁面載入時向 [!DNL Target] Edge 伺服器引發要求。由於全域 mbox 已轉譯為 execute > pageLoad，因此如果您想在頁面載入時觸發要求，便應開啟此設定。

### 即使沒有從 at.js 2.*x* 指定 Target 全域 mbox 名稱，現有的 VEC 活動仍會繼續運作嗎?

會，因為 execute > pageLoad 在 [!DNL Target] 後端上的處理方式如同 `target-global-mbox`。

### 如果我的表單式活動目標鎖定為 `target-global-mbox`，這些活動是否仍會繼續運作?

會，因為 execute > pageLoad 在 [!DNL Target] Edge 伺服器上的處理方式如同 `target-global-mbox`。

### 支援和不支援的 at.js 2.*x* 設定

| 設定 | 支援? |
| --- | --- |
| X-Domain | 無 |
| 自動建立全域 mbox | 是 |
| 全域 mbox 名稱 | 是 |

### at.js 2.x 中的跨網域追蹤支援

跨網域追蹤能夠拼接不同網域之間的訪客。由於必須為各個網域建立新 Cookie，因此在訪客從某個網域導覽至另一個網域時，就很難加以追蹤。為實現跨網域追蹤，[!DNL Target] 使用協力廠商 Cookie 來追蹤網域之間的訪客。這可讓您建立 [!DNL Target] 跨越的活動 `siteA.com` 和 `siteB.com` 和訪客在跨不重複網域導覽時，會維持相同的體驗。 此功能連結至 [!DNL Target]的第三方和第一方Cookie行為。

>[!NOTE]
>
>自at.js 2.10起支援跨網域追蹤，但在at.js 2.*x* 2.10之前。at.js 2.*x* 可透過 Experience Cloud ID (ECID) 資料庫 v4.3.0+ 支援跨網域追蹤。

在 [!DNL Target]，協力廠商Cookie會儲存在 `<CLIENTCODE>.tt.omtrdc.net`.第一方Cookie儲存在 `clientdomain.com`. 第一個要求會傳回 HTTP 回應標頭，此標頭嘗試設定名為 `mboxSession` 和 `mboxPC` 的第三方 Cookie，之後傳回附有額外參數 (`mboxXDomainCheck=true`) 的重新導向要求。若瀏覽器接受協力廠商 Cookie，則重新導向請求會包含這些 Cookie 並傳回體驗。這個工作流程之所以可行，是因為我們使用 HTTP GET 方法。

不過，在at.js 2.*x*，不會使用HTTPGET。 POST而是透過at.js 2.*x*[!DNL Target] 使用 HTTP POST，將 JSON 裝載傳送至 Edge 伺服器。使用HTTPPOST表示檢查瀏覽器是否支援第三方Cookie的重新導向要求將中斷。 這是因為 HTTP GET 要求為等冪交易，而 HTTP POST 是非等冪交易且不得任意重複。因此，at.js 2.*x* （2.10之前）不支援立即可用。 只有 at.js 1.*x* 才提供跨網域追蹤的立即可用支援。

若要針對at.js v2.10或更新版本使用跨網域追蹤，您可以執行下列其中一項作業：

1. 安裝 [ECID資料庫v4.3.0+](https://experienceleague.adobe.com/docs/id-service/using/release-notes/release-notes.html??lang=zh-Hant) 搭配at.js 2.*x*。ECID 資料庫的存在是為了管理可用來識別訪客 (甚至是跨網域) 的持續 ID。安裝 ECID 資料庫 v4.3.0+ 和 at.js 2.*x* 後，您將能建立橫跨多個唯一網域的活動以及追蹤使用者的活動。請務必注意，此功能僅適用於工作階段過期之後。

1. 如果您有at.js v2.10或更新版本，您不必安裝ECID程式庫，而是可以在以下位置啟用跨網域設定： [!DNL Target] 中的UI **[!UICONTROL 管理]** > **[!UICONTROL 實施]**. (或者，您可以設定 _crossDomain_ 選項至 _已啟用_ （在at.js程式碼中）。

若要針對at.js v2版本使用跨網域追蹤。*x* 在2.10之前，您可以實作上述選#1（安裝ECID程式庫）。

### 支援自動建立全域 mbox

此設定可告知 at.js 2.*x* 在頁面載入時向 [!DNL Target] Edge 伺服器引發要求。由於全域 mbox 已轉譯為 execute > pageLoad，且會由 [!DNL Target] Edge 伺服器加以解譯，因此顧客如果希望在頁面載入時觸發要求，便應開啟此功能。

### 支援全域 mbox 名稱

客戶可透過以下方式指定全域mbox名稱： **[!UICONTROL Target]** > **[!UICONTROL 管理]** > **[!UICONTROL 實施]** > **[!UICONTROL 編輯]**. 此設定供 [!DNL Target] Edge 伺服器用來將 execute > pageLoad 轉譯為輸入的全域 mbox 名稱。這麼做可讓客戶繼續使用伺服器端 API、表單式撰寫器、設定檔指令碼，以及建立鎖定全域 mbox 為目標的對象。

### 以下 at.js 自訂事件適用於 `triggerView()`，還是僅適用於 `applyOffer()` 或 `applyOffers()`?

* `adobe.target.event.CONTENT_RENDERING_FAILED`
* `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`
* `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`
* `adobe.target.event.CONTENT_RENDERING_REDIRECT`

是的，at.js 自訂事件也適用於 `triggerView()`。

### 當我打電話時 `triggerView()` 搭配(&amp;L)`"page" : "true"`&amp;rbrace；，它會傳送通知給 [!DNL Target] 後端並增加曝光次數。 那麼也會造成設定檔指令碼執行嗎?

對 [!DNL Target] 後端發出預先擷取呼叫時，會執行設定檔指令碼，接著會加密受影響的設定檔資料，然後傳回用戶端。叫用含有 `{"page": "true"}` 的 `triggerView()` 後，會傳送通知並附上加密的設定檔資料。此時 [!DNL Target] 後端會將設定檔資料解密並儲存至資料庫中。

### 呼叫 `triggerView()` 之前需要新增預先隱藏程式碼以處理忽隱忽現情況嗎?

不需要，您不需要在呼叫 `triggerView()` 之前新增預先隱藏程式碼。at.js 2.*x* 會在顯示和套用檢視之前，處理預先隱藏和忽隱忽現的邏輯。

### 哪個at.js 1.*x* at.js 2.*x* 的 SPA 中)，那麼會發生什麼情況?

下列at.js 1.x引數為 *NOT* 目前支援在使用at.js 2.*x* 中的頁面載入要求:

* browserHeight
* browserWidth
* browserTimeOffset
* screenHeight
* screenWidth
* screenOrientation
* colorDepth
* devicePixelRatio
* vst.*引數（請參閱下文）

### at.js 2.*x* 不支援使用 vst.* 參數

使用at.js 1.*x* 能夠使用vst。* mbox引數以建立對象。 這是at.js 1.*x* 已將mbox引數傳送至 [!DNL Target] 後端。 移轉至at.js 2.*x*，您無法再使用這些引數建立對象，因為at.js 2.*x* 以不同方式傳送mbox引數。

## at.js 相容性

下表說明 at.js。2.*x* 與不同活動型別、整合、功能和at.js函式的相容性。

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
>[!UICONTROL 自動鎖定目標] 活動可透過at.js 2.*x* 和VEC完成所有修改後 `Page Load Event`. 將修改加入至特定檢視時， [!UICONTROL A/B測試]， [!UICONTROL 自動分配]、和 [!UICONTROL 體驗鎖定] 僅支援(XT)活動。

### 整合

| 類型 | 支援? |
| --- | --- |
| [!UICONTROL Analytics for Target] (A4T) | 是 |
| 對象 | 是 |
| 客戶屬性 | 是 |
| AEM 體驗片段 | 是 |
| [Adobe Experience Platform擴充功能](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) | 是 |
| 除錯程式 | 是 |
| Auditor | 尚未針對 at.js 2.*x* 更新規則 |
| 「 」的「選擇加入」支援 [GDPR](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) | 這在中受支援 [at.js 2.1.0版](/help/dev/implement/client-side/atjs/target-atjs-versions.md#atjs-version-210-june-3-2019) 或更新版本。 |
| AEM增強型個人化提供者： [!DNL Adobe Target] | 無 |

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

at.js 2.*x*，就像 at.js 1.*x*，使用自訂事件 `at-request-succeeded` 來呈現回應 Token。如需使用 `at-request-succeeded` 自訂事件的程式碼範例，請參閱[回應 Token](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)。

## at.js 1.*x* 引數重新命名為at.js 2.*x* 的裝載對應

本節概述 at.js 1.*x* 與 at.js 2.*x* 之間的對應。

探索參數對應之前，請注意這些資料庫版本使用的端點已變更:

* at.js 1.*x* - `http://<client code>.tt.omtrdc.net/m2/<client code>/mbox/json`
* at.js 2.*x* - `http://<client code>.tt.omtrdc.net/rest/v1/delivery`

另一項顯著差異為:

* at.js 1.*x* - 用戶端代碼是路徑的一部分
* at.js 2.*x* - 用戶端代碼會以查詢字串參數的形式傳送，例如:
  `http://<client code>.tt.omtrdc.net/rest/v1/delivery?client=democlient`

以下各節列出每個 at.js 1.*x* 引數、其說明以及對應的2。*x* JSON裝載（若適用）：

### at_property

(at.js 1.*x* 參數)

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

(at.js 1.*x* 參數)

頁面網域，其中 [!DNL Target] 程式庫執行。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

瀏覽器的 WEB GL 轉譯器功能我們的裝置偵測機制會使用此功能，判斷訪客的裝置是否為桌上型電腦、iPhone、Android 裝置等。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

頁面 URL。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

頁面轉介者。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

傳遞 API 已無全域 mbox 概念。在 JSON 裝載中，您必須使用 `execute > pageLoad`。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

若要使用 mbox 名稱，請將其傳遞至 `execute > mboxes`。mbox 需要索引和名稱。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

已不再使用。

### mboxCount

(at.js 1.*x* 參數)

已不再使用。

### mboxRid

(at.js 1.*x* 參數)

下游系統用來協助進行偵錯的要求 ID。

at.js 2.*x* JSON 裝載:

```json {line-numbers="true"}
{
  "requestId": "2412234442342"
  ....
}
```

### mboxTime

(at.js 1.*x* 參數)

已不再使用。

### mboxSession

(at.js 1.*x* 參數)

工作階段 ID 會以查詢字串參數 (`sessionId`) 的形式傳送至傳送 API 端點。

### mboxPC

(at.js 1.*x* 參數)

TNT ID 會傳遞至 `id > tntId`。

at.js 2.*x* JSON 裝載:

```json {line-numbers="true"}
{
  "id": {
    "tntId": "ca5ddd7e33504c58b70d45d0368bcc70.21_3"
  }
  ....
}
```

### mboxMCGVID

(at.js 1.*x* 參數)

Marketing Cloud 訪客 ID 會傳遞至 `id > marketingCloudVisitorId`。

at.js 2.*x* JSON 裝載:

```json {line-numbers="true"}
{
  "id": {
    "marketingCloudVisitorId": "797110122341429343505"
  }
  ....
}
```

### `vst.aaaa.id`和`vst.aaaa.authState`

(at.js 1.*x* 參數)

客戶 ID 應傳遞至 `id > customerIds`。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

用來連結不同專案的客戶第三方ID [!DNL Target] ID。

at.js 2.*x* JSON 裝載:

```json {line-numbers="true"}
{
  "id": {
    "thirdPartyId": "1232312323123"
  }
  ....
}
```

### mboxMCSDID

(at.js 1.*x* 參數)

SDID (也稱為補充資料 ID)。應傳遞至 `experienceCloud > analytics > supplementalDataId`。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

[!UICONTROL Analytics 追蹤伺服器。]應傳遞至 `experienceCloud > analytics > trackingServer`。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

Analytics 追蹤伺服器安全。應傳遞至 `experienceCloud > analytics > trackingServerSecure`。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

Audience Manager 位置提示。應傳遞至 `experienceCloud > audienceManager > locationHint`。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

Audience Manager Blob。應傳遞至 `experienceCloud > audienceManager > blob`。

at.js 2.*x* JSON 裝載:

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

(at.js 1.*x* 參數)

版本會透過版本參數以查詢字串參數的形式傳送。

## 訓練影片： at.js 2.*x* 架構圖表 ![總覽徽章](../../assets/overview.png)

at.js 2.*x* 增強Adobe [!DNL Target]的支援SPA，並與其他Experience Cloud解決方案整合。 本影片說明整合方式。

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

另請參閱 [瞭解at.js 2.*x* 工作](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html) 以取得詳細資訊。
