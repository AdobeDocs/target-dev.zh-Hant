---
keywords: adobe.target.trackEvent， trackEvent， trackevent，追蹤事件， at.js，函式，函式， preventDefault， preventdefault，防止預設， adobe.target.trackEvent
description: 使用適用於 [!DNL Adobe Target] at.js JavaScript資料庫的[!UICONTROL adobe.target.trackEvent()]函式來觸發要求，以報告使用者動作，例如您網站的點按和轉換。
title: 如何使用[!UICONTROL adobe.target.trackEvent()]函式？
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 62%

---

# [!UICONTROL adobe.target.trackEvent(options)]

此函數會觸發要求以報告使用者動作，例如點擊和轉換。它不會在回應中傳遞活動。

然後這些事件追蹤 mbox 呼叫可以用來定義活動中的量度。如需詳細資訊，請參閱[成功量度](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)和[追蹤轉換](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)。

以下是 API 詳細資料:

| 索引鍵 | 類型 | 必要 | 說明 |
|--- |--- |--- |--- |
| mbox | 字串 | 是 | Mbox 名稱<P>**注意**：如果[!UICONTROL trackEvent()]呼叫是以已在頁面上觸發的mbox名稱觸發，則[!UICONTROL trackEvent()]的SDID會重設，且將與頁面上的[!DNL Target]呼叫不同。 不過，以不同的mbox名稱引發[!UICONTROL trackEvent()]呼叫，可讓[!UICONTROL trackEvent()]呼叫的SDID與頁面上的[!UICONTROL Page Load Request]/[!UICONTROL triggerView()]呼叫一致。 |
| selector | 字串 | 無 | 用來尋找 HTML 元素的 CSS 選取器。事件接聽程式將附加至找到的元素。 |
| type | 字串 | 無 | 代表已註冊的事件類型。它可以是 HTML 已知事件，如: 按一下、按下滑鼠等等，也可以是自訂 HTML 事件。 |
| preventDefault | 布林值 | 無 | 指出是否在事件接聽程式回呼中使用 `[!UICONTROL event.preventDefault()]`。預設值設為 false。<P>**附註**：只支援`[!UICONTROL form[submit]]`和`a[click]`。 由於複雜度和要支援的案例數量太大，不支援其他案例。 |
| params | 物件 | 無 | mbox 參數.機碼/值組的物件具有下列結構: <P>`{ "param1": "value1", "param2": "value2"}` |
| timeout | 數字 | 無 | 逾時，以毫秒為單位。<P>如果未指定，會使用預設值:<P>`...timeoutInSeconds: 0.15...}` |
| success | 函數 | 無 | 回呼函數，用來指出已報告該事件。 |
| error | 函數 | 無 | 回呼函數，用來指出無法報告該事件。 |

## 範例

```javascript {line-numbers="true"}
<a href="https://asite.com">click me!</a> 
```

加上 javaScript 程式碼來指派 `trackEvent`:

```javascript {line-numbers="true"}
<script> 
$('a').click(function(event){ 
  adobe.target.trackEvent({'mbox':'homePageHero'}) 
}); 
</script> 
```

或:

```javascript {line-numbers="true"}
adobe.target.trackEvent({ 
    "mbox": "clicked-cta", 
    "params": { 
        "param1": "value1" 
    } 
});
```

>[!WARNING]
>
>如果未設定強制欄位，則不會執行任何要求，且會擲回錯誤。
