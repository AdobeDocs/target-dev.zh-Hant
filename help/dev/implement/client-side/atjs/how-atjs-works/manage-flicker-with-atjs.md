---
keywords: 忽隱忽現， at.js，實作，非同步，非同步，同步， $8
description: 瞭解at.js和 [!DNL Target] 如何在頁面或應用程式載入期間防止忽隱忽現（預設內容在被活動內容取代之前暫時顯示）。
title: at.js如何管理忽隱忽現的情形？
feature: at.js
exl-id: 8aacf254-ec3d-4831-89bb-db7f163b3869
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 57%

---

# At.js 處理忽隱忽現情況的方式

有關[!DNL Adobe Target] at.js JavaScript資料庫如何在頁面或應用程式載入期間防止忽隱忽現的資訊。

忽隱忽現是指預設內容在換成活動內容之前短暫顯示給訪客。忽隱忽現不理想，因為可能造成訪客混淆。

## 使用自動建立的全域mbox

若您在設定 at.js 時啟用了[「自動建立全域 Mbox」](/help/dev/implement/client-side/atjs/global-mbox/customize-global-mbox.md)設定，at.js 會透過變更頁面載入時的不透明度設定，來處理忽隱忽現的情形。at.js載入時，會將`<body>`元素的不透明度設定變更為「0」，使訪客一開始無法看到頁面。 收到來自[!DNL Target]的回應之後，或偵測到[!DNL Target]要求的錯誤時，at.js會將不透明度重設為「1」。 這種做法可確保訪客只能在活動內容套用之後才看得到頁面。

如果您在設定 at.js 時啟用此設定，則 at.js 會將 HTML BODY 樣式不透明度設定為 0。收到來自[!DNL Target]的回應後，at.js會將HTMLBODY不透明度重設為1。

將不透明度設為 0，可隱藏頁面內容以避免忽隱忽現的情形發生，但瀏覽器仍會輸出頁面，並載入 CSS、影像等所有必要資產。

如果`opacity: 0`在您的實作中無法運作，您也可以透過自訂`bodyHiddenStyle`並將其設定為`body {visibility:hidden !important}`來管理忽隱忽現的問題。 您可以使用最適合您特定環境的`body {opacity:0 !important}`或`body {visibility:hidden !important}`。

下圖顯示 js 1.*x* 和 at.js 2.x。

**at.js 2.x**

（按一下影像可展開至完整寬度。）

![Target流程： at.js頁面載入要求](/help/dev/implement/client-side/assets/atjs-20-flow-page-load-request.png "Target流程： at.js頁面載入要求"){zoomable="yes"}

**at.js 1.*x***

（按一下影像可展開至完整寬度。）

![目標流程：自動建立的全域mbox](/help/dev/implement/client-side/atjs/how-atjs-works/assets/target-flow2.png "目標流程：自動建立的全域mbox"){zoomable="yes"}

如需 `bodyHiddenStyle` 覆寫的詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。

## 處理非同步載入 at.js 時忽隱忽現的情形

非同步載入 at.js 可有效避免讓瀏覽器無法呈現；不過，此技術可能導致網頁上忽隱忽現。

您可以使用預先隱藏的程式碼片段來避免忽隱忽現情形，此片段要等到Target將相關的HTML元素個人化之後才可見。

at.js可非同步載入，直接內嵌於頁面或透過標籤管理員(例如Adobe Experience Platform Launch)進行。

如果頁面上內嵌at.js，則必須先新增程式碼片段，才能載入at.js。 如果您透過標籤管理員載入at.js （也以非同步方式載入），您必須在載入標籤管理員之前新增程式碼片段。 如果同步載入標籤管理員，指令碼可能會包含在at.js之前的標籤管理員中。

預先隱藏的程式碼片段如下所示:

```
;(function(win, doc, style, timeout) {
  var STYLE_ID = 'at-body-style';

  function getParent() {
    return doc.getElementsByTagName('head')[0];
  }

  function addStyle(parent, id, def) {
    if (!parent) {
      return;
    }

    var style = doc.createElement('style');
    style.id = id;
    style.innerHTML = def;
    parent.appendChild(style);
  }

  function removeStyle(parent, id) {
    if (!parent) {
      return;
    }

    var style = doc.getElementById(id);

    if (!style) {
      return;
    }

    parent.removeChild(style);
  }

  addStyle(getParent(), STYLE_ID, style);
  setTimeout(function() {
    removeStyle(getParent(), STYLE_ID);
  }, timeout);
}(window, document, "body {opacity: 0 !important}", 3000));
```

依預設，此程式碼片段會預先隱藏整個 HTML BODY。在某些情況下，您可能只想預先隱藏特定的 HTML 元素，而非整個頁面。您可以自訂 style 參數來達到此目的。此參數可以換成某些項目，而只預先隱藏頁面的特定區域。

例如，您有兩個區域，分別以 ID container-1 和 container-2 來識別，則 style 可以換成如下內容:

```
#container-1, #container-2 {opacity: 0 !important}
```

並非預設值:

```
body {opacity: 0 !important}
```

## 針對triggerView()管理at.js 2.x中的忽隱忽現問題

使用 `triggerView()` 顯示 SPA 中的目標內容時，會自動提供處理忽隱忽現問題的功能。這表示不需要手動新增預先隱藏邏輯。at.js 2.x 會在套用目標內容之前，預先隱藏需要顯示檢視的位置。

## 使用getOffer()和applyOffer()管理閃爍

因為 `getOffer()` 和 `applyOffer()` 都是低階 API，並沒有內建的忽隱忽現控制項。您可以將選取器或 HTML 元素當作選項傳給 `applyOffer()`，在此情況下，`applyOffer()` 會將活動內容新增至特定元素，不過在叫用 `getOffer()` 和 `applyOffer()` 之前，您必須確定元素已適當隱藏。

```
document.documentElement.style.opacity = "0";
 
adobe.target.getOffer({
    mbox: 'target-global-mbox',
    success: function(offer) {
        adobe.target.applyOffer({
            mbox: 'target-global-mbox',
            offer: offer
        });
 
        document.documentElement.style.opacity = "1";
    },
    error: function() {
        document.documentElement.style.opacity = "1";        
    }
});
```

## 透過at.js 1.x中的mboxCreate()使用地區mbox （at.js 2.x不支援）

如果您使用地區 mbox 實作，則可以使用 `mboxCreate()` 搭配您佈建的頁面，類似下列範例程式碼:

```
<div class="mboxDefault">
Some default content
</div>
<script>
mboxCreate('some-mbox');
</script>
```

如果頁面適當地佈建，at.js 會在含有 mboxDefault 類別的元素上切換 CSS「visibility」屬性，以處理忽隱忽現問題。
