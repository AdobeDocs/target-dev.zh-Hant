---
keywords: 預先隱藏SDK，忽隱忽現，防忽隱忽現，預先隱藏，預先隱藏， alloy， at.js，實作，同意， CMP，指令碼位置，內嵌，外部， SDK選擇
description: 瞭解如何整合 [!DNL Adobe Target] 預先隱藏SDK以消除頁面載入期間非個人化內容的閃爍（閃爍）。 SDK可與Adobe Alloy （網頁SDK）和at.js搭配使用。
title: 預先隱藏SDK整合指南
feature: Implementation
hide: true
source-git-commit: 2f7a53b667990474dfab7ca66a8ea93d2e946548
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 0%

---


# 預先隱藏SDK整合指南

小型同步JavaScript程式庫，可防止[!DNL Adobe Target]個人化在轉譯器期間重寫頁面所導致的視覺閃爍。 在`<head>`頂端新增一個內嵌`<script>`標籤，且頁面只有在個人化內容準備就緒或安全計時器觸發後才會顯示。

## 整合步驟 {#integration-steps}

1. 下載套件組合。

   使用Flicker Manager UI下載`prehide.min.js`。 檔案已預先設定您的使用者端代碼和保護逾時，因此不需要`PrehideConfig`區塊。

1. 在`<head>`的最上方內嵌它。

   將`prehide.min.js`的內容直接貼到內嵌`<script>`標籤中，做為`<head>`的第一個子系。 請參閱[內嵌與外部](#inline-vs-external)，瞭解為何偏好使用內嵌。

   ```html
   <!-- 1. Prehide SDK: must be FIRST in <head> and BEFORE any Adobe SDK -->
   <script>
     // paste the contents of prehide.min.js here
   </script>
   
   <!-- 2. Then load Alloy.js OR at.js -->
   <script src="https://your-cdn/alloy.min.js"></script>
   ```

1. （選用）新增執行階段設定區塊。

   只有在您自行託管套件組合而未透過UI下載時，或需要覆寫SDK選擇時，才需要使用此專案。 將設定區塊放在預先隱藏指令碼之前：

   ```html
   <script>
     window.PrehideConfig = {
       sdk: "alloy"            // or "atjs" (defaults to "alloy")
     };
   </script>
   <script> /* prehide.min.js inline contents */ </script>
   <script src="https://your-cdn/alloy.min.js"></script>
   ```

1. （選用）通知同意。

   如果您的實作使用同意管理平台(CMP)，請在知道同意狀態後立即呼叫`window.Prehide.setConsent(...)`。 請參閱[同意管理](#consent-management)。

1. 驗證。

   開啟DevTools，並確認第一次繪圖時`<head>`中出現`<style id="alloy-prehiding">` （或at.js的`at-body-style`），並在SDK完成後移除。 在主控台中執行`window.Prehide.getState()`以檢查執行階段狀態。

## 放置指令碼的位置 {#script-placement}

>[!IMPORTANT]
>
>預先隱藏SDK必須在Alloy/at.js之前執行。如果Alloy先載入，頁面會呈現非個人化內容，然後重新呈現。這正是此SDK設計用來防止的閃爍。
></br>>請勿將`async`或`defer`新增至「預先隱藏SDK」指令碼標籤。需要同步執行，以便在瀏覽器開始佈局頁面之前插入隱藏規則。

預先隱藏SDK在檔案中出現的時間必須早於在其之後清理的[!DNL Adobe Target] SDK。 載入順序不可協商：

```html
<!doctype html>
<html>
<head>
  <!-- ① Prehide SDK FIRST. Inline. Synchronous. No async/defer. -->
  <script> /* prehide.min.js inline contents */ </script>

  <!-- ② Alloy or at.js loads next -->
  <script src="https://cdn.alloy.adobe.com/alloy.min.js"></script>

  <!-- ③ Then everything else: meta, css, third-party tags, ... -->
  <link rel="stylesheet" href="main.css">
</head>
<body> ... your page ... </body>
</html>
```

## 內嵌與外部 {#inline-vs-external}

包含`prehide.min.js`的方式有兩種：

| 方法 | 範例 | 附註 |
| --- | --- | --- |
| 內嵌（偏好設定） | `<script>/* full contents of prehide.min.js pasted directly into the page */</script>` | 零網路來回次數。 SDK會在轉譯其他任何內容之前執行。 |
| 外部（只有在無法內嵌的情況下） | `<script src="/static/prehide.min.js"></script>` | 在首次轉譯之前引入封鎖網路要求。 即使使用HTTP/2和邊緣快取，通常也需要30-80毫秒。 |

### 為何偏好使用內嵌

>[!TIP]
>
>在您的HTML範本中直接在`<script>...</script>`內巢狀件組合。 將其視為關鍵CSS區塊：小、內嵌，並一律位於最上方。

* 沒有轉譯封鎖擷取。 預先隱藏SDK的目的是要在第一次轉譯前&#x200B;*插入隱藏規則*。 外部`<script src>`會在該關鍵視窗中新增網路來回次數。
* 沒有新的失敗模式。 外部檔案可能為404、逾時或被廣告封鎖程式封鎖。 無法進行內嵌複製。
* 組合很小(~6 KB)。 內嵌成本比一般的Favicon還低，而且快取效益也不夠大，不足以超過初次轉譯時額外的往返次數。
* 適合快取。 在HTML回應中內聯時，現有快取層（CDN或瀏覽器HTTP快取）會將SDK與檔案的其餘部分一起快取。
* 客戶專屬套件。 下載的檔案在下載時已將您的使用者端代碼存入。 內嵌可確保每位訪客都能收到正確的自訂套件，不需要其他請求。

## 設定 {#configuration}

SDK接受來自兩個來源的設定，依優先順序排列。 它會讀取最先提供的任何專案。

### 執行階段`window.PrehideConfig` （手動整合）

對於自行託管或未修改的組合，請在預先隱藏指令碼執行之前宣告設定物件：

```html
<script>
  window.PrehideConfig = {
    sdk: "alloy"             // optional: "alloy" (default) or "atjs"
  };
</script>
```

| 欄位 | 類型 | 必要 | 說明 |
| --- | --- | --- | --- |
| `sdk` | `"alloy"` \| `"atjs"` | 否 | 頁面上載入Adobe SDK。 請參閱[SDK選取專案](#sdk-selection)。 |

## SDK選取範圍 {#sdk-selection}

[!DNL Adobe Target]提供兩種傳遞SDK： Alloy （現代的Web SDK）和at.js （傳統資料庫）。 個人化完成時，每個都會尋找&#x200B;*不同的* `<style>`元素`id`，並將其移除以顯示頁面。 預先隱藏SDK必須插入相符的`id`；否則頁面會保持隱藏狀態，直到安全計時器觸發為止。

| `sdk`值 | 插入樣式標籤ID | 移除者 | 使用時機 |
| --- | --- | --- | --- |
| `"alloy"` *（預設）* | `<style id="alloy-prehiding">` | Alloy SDK on personalization-complete | 您正在本頁載入Alloy / Adobe Web SDK。 |
| `"atjs"` | `<style id="at-body-style">` | Personalize-complete上的at.js | 您正在載入此頁面上的傳統at.js資料庫。 |

>[!NOTE]
>
>若是at.js SDK，僅支援2.x版及更新版本。

### 如何設定

```html
<!-- For at.js -->
<script>
  window.PrehideConfig = { sdk: "atjs" };
</script>
<script> /* prehide.min.js inline */ </script>
<script src="https://cdn.adobe.com/.../at.js"></script>
```

>[!WARNING]
>
>不符警告。 載入at.js時設定`sdk: "alloy"` （反之亦然）表示SDK找不到要移除的預先隱藏專案。 防護計時器最終會顯示頁面，但訪客會體驗到較長的隱藏視窗。 一律設定`sdk`以符合您載入的程式庫。

未知或遺失的值會回覆為`"alloy"`，因此現有的Alloy整合可繼續運作，而不會變更任何組態。

## 同意管理 {#consent-management}

>[!NOTE]
>
>* 同意值絕對不會儲存在`window`上。 只會公開函式；內部狀態對SDK而言仍屬私有。
>* 從`"out"`到`"in"`的轉換不會重新隱藏頁面，因為重新隱藏完全演算的內容在視覺上可能會造成中斷。
>* 可以在單一頁面檢視中多次呼叫`setConsent`。 每個呼叫都會取代先前的狀態。

預先隱藏SDK包含同意感知的API，可與您的CMP協調。 使用它是選用的。 如果從未呼叫`setConsent`，則SDK的行為就像標準的未經同意整合。

### API表面

```js
// Single function call. Pass a status string.
window.Prehide.setConsent("pending");  // banner shown, awaiting decision
window.Prehide.setConsent("in");       // user accepted personalization
window.Prehide.setConsent("out");      // user declined personalization

// Read-only debug snapshot.
window.Prehide.getState();
// → { sdk, version, consentStatus, consentApiUsed,
//      rulesResolved, hasSelectorsToGuard, guardTimeout }
```

### 每個狀態的功能

| 呼叫 | 保護計時器上的效果 | 對隱藏內容的影響 |
| --- | --- | --- |
| `setConsent("pending")` | 已清除作用中的計時器。 在解決同意之前，沒有任何安全性會解除隱藏火災。 | 選取器會無限期隱藏。 |
| `setConsent("in")` | 已清除，然後在設定的逾時下重新啟動。 如果規則尚未解析，會等待規則解析。 | 內容會保持隱藏狀態，直到SDK個人化或防護計時器引發為止。 |
| `setConsent("out")` | 已清除。 頁面會立即取消隱藏。 | 頁面會立即顯示。 稍後解析的規則&#x200B;*不會*&#x200B;重新隱藏內容。 |
| *（從未呼叫）* | 預設計時器會從`init()`執行設定的持續時間。 | 內容會保持隱藏狀態，直到SDK個人化或防護計時器引發為止。 （回溯相容舊模式。） |

### 建議的模式：明確的待定階段

1. 當您的CMP顯示同意UI時，請呼叫`setConsent("pending")`。 這樣會清除安全計時器，所以在訪客決定頁面時頁面會保持隱藏，以防止橫幅後面出現非個人化內容的閃爍。

   ```js
   window.Prehide.setConsent("pending");
   ```

1. 當訪客接受個人化時，請呼叫`setConsent("in")`。 「保全」計時器會重新啟動，且Alloy/at.js會接管，在應用個人化後會顯示頁面。

   ```js
   window.Prehide.setConsent("in");
   ```

1. 當訪客拒絕個人化時，請致電`setConsent("out")`。 此頁面會立即顯示並維持可見。 稍後解析的CDN規則將不會重新隱藏它。

   ```js
   window.Prehide.setConsent("out");
   ```

### 範例：OneTrust樣式整合

```js
// Called once OneTrust has rendered its banner.
function onOneTrustReady() {
  // Pause the guard timer while the banner is visible.
  window.Prehide.setConsent("pending");

  OneTrust.OnConsentChanged(function (event) {
    if (event.consentedToTargeting) {
      window.Prehide.setConsent("in");
    } else {
      window.Prehide.setConsent("out");
    }
  });
}
```

### 範例： TCF / IAB樣式

```js
// Optional: pause the guard while UI is up.
window.Prehide.setConsent("pending");

// Your CMP eventually emits the final TC string.
function onTcData(tcData) {
  const hasTargetConsent = /* derive from tcData */;
  window.Prehide.setConsent(hasTargetConsent ? "in" : "out");
}
```

