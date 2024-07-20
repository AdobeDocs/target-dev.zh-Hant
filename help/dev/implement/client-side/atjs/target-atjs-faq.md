---
keywords: at.js faq, at.js 常見問答集, flicker, loader, 頁面載入器, 跨網域, 檔案大小, x-網域, at.js 與 mbox.js, 僅限 x, safari, 單一頁面應用程式, 缺少選取器, 選取器, tt.omtrdc.net, spa, Adobe Experience Manager, AEM, ip 位址, httponly, HttpOnly, secure, ip, cookie 網域
description: 閱讀有關 [!DNL Adobe Target] at.js JavaScript資料庫的常見問題解答。
title: 關於at.js有哪些常見問答？
feature: at.js
exl-id: 362ccc5b-8731-46c0-bc52-3e55c273e216
source-git-commit: 448c43c0c10e22ad054f4ee98bfc282f8c96cdcb
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 39%

---

# at.js 常見問題

有關[!DNL Adobe Target] at.js JavaScript資料庫的常見問題解答。

## 使用at.js或mbox.js各有何優點？

at.js程式庫會取代mbox.js。 不再支援mbox.js資料庫。 不過，對於大多數人來說，at.js的優點多於mbox.js。

除了眾多優點以外，at.js還能改進Web實施的頁面載入時間、改進安全性，以及為單頁應用程式提供更好的實施選項。

下圖比較使用 mbox.js 或 at.js 的頁面載入效能。

（按一下影像可展開至完整寬度。）

![比較mbox.js與at.js的頁面效能圖表](/help/dev/implement/client-side/atjs/assets/atjs_versus_mboxjs.png "比較mbox.js與at.js的頁面效能圖表"){zoomable="yes"}

如上圖所示，使用mbox.js時，頁面內容直到[!DNL Target]呼叫完成之後才會開始載入。 使用 at.js 時，頁面內容在 [!DNL Target] 呼叫起始時就開始載入，不會等到呼叫完成。

## at.js和mbox.js對頁面載入時間有何影響？

許多客戶和顧問都想要瞭解at.js和mbox.js對頁面載入時間的影響，尤其是在比較新使用者和再度訪問的使用者時。 可惜，有關at.js或mbox.js如何影響頁面載入時間，由於每個客戶的實施不同，很難測量和提出具體數字。

不過，如果頁面上有「訪客API」，[!DNL Target]就能更充分的瞭解at.js和mbox.js如何影響頁面載入時間。

>[!NOTE]
>
>只有當使用全域mbox時，「訪客API」和at.js或mbox.js才會影響頁面載入時間（原因在於主體隱藏技術）。 訪客 API 整合不影響地區 mbox。

以下小節說明新訪客和再度造訪的訪客的動作序列:

### 新訪客

1. 訪客 API 會載入、剖析和執行。
1. at.js / mbox.js 會載入、剖析和執行。
1. 如果已啟用全域mbox自動建立，[!DNL Target] JavaScript程式庫：

   * 實體化訪客物件。
   * [!DNL Target]資料庫嘗試擷取Experience Cloud訪客ID資料。
   * 由於此訪客是新訪客，因此訪客API會向demdex.net發出跨網域請求。
   * 擷取Experience Cloud訪客ID資料後，會觸發對[!DNL Target]的請求。

### 再度訪問的訪客

1. 訪客 API 會載入、剖析和執行。
1. at.js / mbox.js 會載入、剖析和執行。
1. 如果已啟用全域mbox自動建立，[!DNL Target] JavaScript程式庫：

   * 實體化訪客物件。
   * [!DNL Target]資料庫嘗試擷取Experience Cloud訪客ID資料。
   * 訪客 API 會從 Cookie 擷取資料。
   * 擷取Experience Cloud訪客ID資料後，會觸發對[!DNL Target]的請求。

>[!NOTE]
>
>對於新訪客，當訪客API出現時，[!DNL Target]必須多次透過線路以確保[!DNL Target]請求包含Experience Cloud的訪客ID資料。 若為回訪訪客，[!DNL Target]僅會透過線路傳送至[!DNL Target]以擷取個人化內容。

## 從舊版 at.js 升級為版本 1.0.0 之後，為什麼我發現回應時間好像變慢了?

at.js 1.0.0版和更新版本會平行觸發所有請求。 舊版會循序執行請求，亦即請求會放入佇列中，[!DNL Target]會等待第一個請求完成後再繼續處理下一個請求。

舊版at.js執行請求的方式易於發生所謂的「線頭阻塞」。 在at.js 1.0.0和更新版本中，[!DNL Target]已切換為平行請求執行。

如果您檢查at.js 0.9.1的網路標籤瀑布圖，則會看到下一個[!DNL Target]請求要等到上一個完成時才會開始。 at.js 1.0.0和更新版本就不是這個序列，所有要求基本上是同時開始。

從回應時間的角度，從數學上來說，這個順序可以這樣加總

<ul class="simplelist"> 
 <li> at.js 0.9.1：所有[!DNL Target]個要求的回應時間=要求回應時間總和 </li> 
 <li> at.js 1.0.0和更新版本：所有[!DNL Target]個要求的回應時間=要求回應時間的最大值 </li> 
</ul>

at.js程式庫1.0.0版可更快完成要求。 此外，at.js要求為非同步，因此[!DNL Target]不會阻擋頁面呈現。 即使要求需要幾秒鐘才能完成，您仍會看到轉譯的頁面，在[!DNL Target]從[!DNL Target]邊緣收到回應之前，頁面只有一些部分空白。

## 我可以非同步載入[!DNL Target]資料庫嗎？

at.js 1.0.0版可讓您非同步載入[!DNL Target]資料庫。

若要非同步載入 at.js:

* 建議方法是透過Adobe Experience Platform中的標籤進行。
* 您也可以在載入 at.js 的指令碼標記中新增 async 屬性，就能非同步載入 at.js。使用類似以下的內容：

  ```
  <script src="<URL to at.js>" async></script>
  ```

* 您也可以使用此程式碼來非同步載入 at.js:

  ```
  var script = document.createElement('script'); 
  script.async = true; 
  script.src = "<URL to at.js>"; 
  document.head.appendChild(script);
  ```

非同步載入 at.js 可有效避免讓瀏覽器無法呈現；不過，此技術可能導致網頁上忽隱忽現。

您可以使用預先隱藏的程式碼片段來避免忽隱忽現情形，它會隱藏頁面（或指定部分），然後在at.js和全域要求載入後顯示內容。 您必須在載入 at.js 之前新增此程式碼片段。

如果您是透過非同步[!UICONTROL Adobe Experience Platform]實作部署at.js，請使用[!UICONTROL Adobe Experience Platform]內嵌程式碼實作[!DNL Target]，並確實在頁面上直接包含預先隱藏程式碼片段。

如果您是透過同步 DTM 實作部署 at.js，可透過頁面頂端觸發的頁面載入規則新增預先隱藏的程式碼片段。

如需詳細資訊，請參閱 [at.js 處理忽隱忽現情況的方式](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)。

## at.js是否與[!DNL Adobe Experience Manager]整合(Experience Manager)相容？

具有FP-11577 （或更新版本）的[!DNL Adobe Experience Manager] 6.2現在支援at.js實作與其[!UICONTROL Adobe Target Cloud Services]整合。

## 使用at.js時如何防止頁面載入忽隱忽現？

[!DNL Target]提供數種方法來防止頁面載入忽隱忽現。 如需詳細資訊，請參閱[使用 at.js 防止忽隱忽現情形](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)。

## at.js的檔案大小多大？

at.js 檔案下載後大約 109 KB。不過，因為大部分伺服器會自動壓縮檔案，使檔案變小，at.js 在伺服器上壓縮 (使用 GZIP 或其他方法) 和當使用者造訪您的網站而載入時，大約是 34 KB。您安裝 at.js 的伺服器上的壓縮設定，就決定實際壓縮大小。

## at.js為何比mbox.js還大？

at.js實作使用單一資料庫(at.js)，而mbox.js實作實際上使用兩個資料庫（mbox.js和target.js）。 所以，at.js 要同時與 mbox.js *和* `target.js` 一起比較才公平。比較兩個版本的 gzip 大小，at.js 1.2 版是 34 KB，而 mbox.js 63 版是 26.2 KB。

at.js 較大，因為它執行的 DOM 剖析比 mbox.js 多很多。這有必要，因為 at.js 在 JSON 回應中是取得「原始」資料，必須轉換成有意義的資料。mbox.js使用`document.write()`，所有剖析均由瀏覽器完成。

儘管檔案較大，但我們的測試指出 at.js 的頁面載入比 mbox.js 更快。此外，在安全性方面，at.js較優秀，因為它不會動態載入其他檔案或使用`document.write`。

## at.js 中有 jQuery 嗎? 因為我的網站上已有jQuery，我可以移除at.js的這部分嗎？

at.js目前使用部分的jQuery，因此您在at.js頂端會看到MIT授權通知。 jQuery 不公開，不會干擾您在頁面上已有的 jQuery 程式庫 (可能是不同版本)。恕不支援移除 at.js 內的 jQuery 程式碼。

## at.js支援Safari和設為x-only的跨網域嗎？

否，如果跨網域設為x-only且Safari已停用第三方Cookie，則mbox.js和at.js會設定停用的Cookie，而且不會針對該特定使用者端的網域執行任何mbox要求。

為了支援Safari訪客，較好的X-Domain應該是「停用」（僅設定第一方Cookie）或「啟用」（僅在Safari上設定第一方Cookie，在其他瀏覽器上設定第一方和第三方Cookie）。

## 我可以在單頁應用程式中使用目標[!UICONTROL Visual Experience Composer] (VEC)嗎？

可以，若您使用at.js 2.x，便可針對SPA使用VEC。如需詳細資訊，請參閱[單頁式(SPA)視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/spa-visual-experience-composer.html)。

## 我可以對 at.js 實施使用 Adobe Experience Cloud Debugger 嗎?

是.您也可以使用 mboxTrace 以進行偵錯，或使用瀏覽器的開發人員工具以檢查網路請求，篩選「mbox」以隔離 mbox 呼叫。

## 對於 at.js，mbox 名稱中可以使用特殊字元嗎?

可以，與 mbox.js 相同。

## 為什麼在我的網頁上 mbox 不會觸發?

[!DNL Target]客戶有時會使用包含[!DNL Target]的雲端型執行個體來進行測試或簡單的概念證明用途。 這些網域和許多其他網域均屬於[公用字尾清單](https://publicsuffix.org/list/public_suffix_list.dat)。

如果您使用這些網域，則新式瀏覽器不會儲存Cookie，除非您使用targetGlobalSettings()自訂`cookieDomain`設定。 如需詳細資訊，請參閱[搭配 [!DNL Target]](/help/dev/implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md)使用雲端型執行個體。

## 使用 at.js 時，可以將 IP 位址用作 Cookie 網域嗎?

可以，只要您使用 [at.js 1.2 版或更新版本](/help/dev/implement/client-side/atjs/target-atjs-versions.md)。不過，Adobe強烈建議您保持最新版本。

>[!NOTE]
>
>如果您使用 at.js 1.2 版或更新版本，則不需要下列範例。

視您使用 [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) 的方式而定，在下載 at.js 之前，您可能需要另外修改程式碼。例如，假設您的 [!DNL Target] 實施在各種網站上需要稍微不同的設定，且您無法使用自訂 JavaScript 來動態定義這些設定，請在下載檔案之後和上傳至個別網站之前，手動完成這些自訂。

下列範例可讓您使用 `targetGlobalSettings()` at.js 函數，插入程式碼片段來支援 IP 位址:

此範例適用於單一 IP 位址:

```
if (window.location.hostname === '123.456.78.9') { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

此範例適用於 IP 位址範圍:

```
if (/^123\.456\.78\..*/g.test(window.location.hostname)) { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

## 為何會看到警告訊息，例如「動作缺少選取器」?

這些訊息與at.js功能無關。 at.js程式庫會嘗試報告DOM中找不到的任何事物。

如果您看到此警告訊息，以下為可能的根本原因:

* 頁面正在動態建立，且at.js找不到元素。
* 頁面建置速度緩慢（因為網路速度緩慢），且at.js在DOM中找不到選取器。
* 執行活動的頁面結構已變更。如果您在可視化體驗撰寫器 (VEC) 中重新開啟活動，應該會看到警告訊息。更新活動，以便找到所有必要的元素。
* 基礎頁面是單頁應用程式(SPA)的一部分，或頁面包含的元素出現在頁面很下方，而at.js「選取器輪詢機制」找不到這些元素。 提高 `selectorsPollingTimeout` 或許有用。如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。
* 任何點擊追蹤量度會嘗試將本身新增至每個頁面，而不論設定此量度的 URL。此狀況雖然無害但會顯示許多這類訊息。

  為了獲得最佳結果，請下載並使用[最新版本的at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md)。 如需有關如何下載at.js的詳細資訊，請參閱&#x200B;[*如何部署at.js* > *實作[!DNL Target]而不使用標籤管理員*](how-to-deployatjs/implement-target-without-a-tag-manager.md)&#x200B;文章中的[使用 [!DNL Target] 介面下載at.js](how-to-deployatjs/implement-target-without-a-tag-manager.md#download-atjs-using-the-target-interface)區段。

## [!DNL Target]伺服器呼叫的目標網域tt.omtrdc.net為何？

tt.omtrdc.net是AdobeEDGE網路的網域名稱，用來接收[!DNL Target]的所有伺服器呼叫。

## 為何at.js不一律使用HttpOnly和Secure Cookie標幟？

HttpOnly 只能透過伺服器端編碼來設定。[!DNL Target] Cookie （例如mbox）是透過JavaScript程式碼建立和儲存的，因此[!DNL Target]無法使用HttpOnly Cookie標幟。 啟用跨網域時，[!DNL Target]對從伺服器端設定的第三方Cookie不使用set HttpOnly。

Secure 只有在頁面是經由 HTTPS 來載入時，能透過 JavaScript 設定。如果頁面一開始是透過 HTTP 載入，JavaScript 無法設定此旗標。此外，如果使用Secure標幟，則Cookie僅可在HTTPS頁面上使用。 對於透過HTTPS載入的頁面，[!DNL Target]會設定Secure和SameSite=None屬性。

為了確保[!DNL Target]可以正確追蹤使用者，而且因為Cookie是在使用者端產生，[!DNL Target]除了上述情況外，不會使用任何這些標幟。

## at.js如何處理安全性問題，例如XSS和MITM攻擊？

只要targetGlobalSettings()函式([targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md))中的`secureOnly`選項設為true，與at.js所啟用的Adobe Edge網路通訊就只能透過HTTPS進行，否則at.js可根據頁面通訊協定，在HTTP與HTTPS之間切換。

預設會強制執行下列標頭：
* HTTP強制安全傳輸技術(HSTS)
* X-XSS保護
* X內容型別選項
* 反向連結原則

已用於使用者端頁面的所有標頭皆可強制執行。 一個常見方法是透過「HTTP要求標頭授權」。 Adobe客戶服務可就最佳方法和實務提供進一步建議。

此外，對Adobe Edge網路的請求是公開的（因為這些請求是設計成從訪客的瀏覽器中發出），並且不包含可見的訪客詳細資訊（僅包含訪客ID）。 這些請求會將體驗傳送給訪客，而且包含訪客應在頁面上看到的詳細資訊。

請注意，對於在這些要求中傳輸的回應Token和工作階段ID：

* 他們追蹤通訊工作階段
* 由隨機字元組成
* 工作階段ID的有效時間為30分鐘
* 回應Token可以停用（[回應Token](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)）
* 它們僅在Adobe解決方案的環境中有用。

在at.js要求中，應該會看到值為「*」的`Access-Control-Allow-Origin`標頭，因為它們是公開的，不需要驗證，而且需要透過JavaScript呼叫從任何網域存取Adobe Edge網路。

不過，必須在頁面上執行內容安全性原則(CSP)。 如需at.js CSP需求的詳細資訊，請參閱[內容安全性原則](/help/dev/before-implement/privacy/content-security-policy.md)和[targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。

## at.js 觸發網路要求的頻率為何?

[!DNL Target]會在伺服器端執行其所有決策。 這表示 at.js 會在每次頁面重新載入或叫用 at.js 公用 API 時，觸發網路要求。

## 在最好的情況下，我們能否期望使用者在隱藏、取代，和顯示內容方面，不會受到任何頁面載入上的可見影響?

at.js會試圖長時間避免預先隱藏HTMLBODY或其他DOM元素，但這取決於網路條件和活動設定。 at.js 提供的[設定](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)可用於自訂隱藏 CSS 樣式的 BODY 設定，如此一來您就可以僅預先隱藏頁面的某些部份，而不用隱藏整個 HTML BODY。期望是那些部分包含了必須「個人化」的 DOM 元素。

## 在使用者符合活動資格的一般情況下，事件的序列為何?

由於 at.js 要求為非同步 `XMLHttpRequest`，因此我們會執行下列步驟:

1. 頁面載入。
1. at.js 預先隱藏 HTML BODY。有可以預先隱藏特定容器，而非 HTML BODY 的設定。
1. at.js 要求觸發。
1. 收到[!DNL Target]回應後，[!DNL Target]會擷取CSS選取器。
1. 使用CSS選取器，[!DNL Target]會建立STYLE標籤，以預先隱藏將會自訂的DOM元素。
1. 移除預先隱藏 STYLE 的 HTML BODY。
1. [!DNL Target]開始輪詢DOM元素。
1. 如果找到DOM元素，[!DNL Target]會套用DOM變更，並移除元素預先隱藏的STYLE。
1. 如果找不到DOM元素，全域逾時會取消隱藏這些元素，以避免頁面損毀。

## at.js最後取消隱藏活動正在變更的元素時，頁面內容完全載入和顯示的頻率為何？

考量到上述情況，at.js最後取消隱藏活動正在變更的元素時，頁面內容完全載入和顯示的頻率為何？ 換句話說，除了活動內容外，頁面會完全顯示，而活動內容則會在其餘內容顯示後稍微顯示。

at.js 不會讓頁面無法呈現。使用者可能會注意到頁面上的一些空白區域代表[!DNL Target]自訂的元素。 若要套用的內容並未包含許多遠端資產 (例如 SCRIPT 或 IMG)，則所有內容皆應快速呈現。

## 完全快取的頁面會如何影響上述情況? 其餘頁面內容載入後，活動內容是否更有可能明顯可見?

如果在靠近使用者位置但不靠近[!DNL Target]邊緣的CDN上快取頁面，則該使用者可能會看到一些延遲。 [!DNL Target]個邊緣分佈於全球各地，因此這通常不是問題。

## 是否可以顯示主圖影像，然後在短暫的延遲後進行更換?

考量到下列情況:

[!DNL Target]逾時為5秒。 使用者載入具有自訂主圖影像活動的頁面。at.js 傳送要求以確認是否有要套用的活動，但無初始回應。假設使用者看見了主圖影像的一般內容，因為並未從[!DNL Target]收到任何關於相關活動是否存在的回應。 四秒後，[!DNL Target]會傳回包含活動內容的回應。

在此階段，是否可以顯示替代版本? 4 秒後，是否可以更換主圖影像且使用者會注意到此影像更換?

起初，主圖影像 DOM 元素會隱藏。收到來自[!DNL Target]的回應後，at.js會套用DOM變更（例如取代IMG並顯示自訂的主圖影像）。

## at.js 需要什麼 HTML doctype?

at.js 需要 HTML 5 doctype。

此語法為:

`<!DOCTYPE html>`

HTML 5 doctype 可確保頁面以標準模式載入。使用怪異模式載入時，會停用 at.js 所根據的某些 JS API。[!DNL Target]會在Quirks模式中停用at.js。

## at.js是否適用於Ionic應用程式環境。

此實作從未進行過測試，因為at.js的用途並非在非Web環境中運作。 [!DNL Adobe]建議將其[SDK用於行動實作](/help/dev/implement/mobile/overview.md)。
