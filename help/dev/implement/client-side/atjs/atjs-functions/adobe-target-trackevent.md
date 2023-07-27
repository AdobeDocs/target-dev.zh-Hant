---
keywords: adobe.target.trackEvent， trackEvent， trackevent，追蹤事件， at.js，函式，函式， preventDefault， preventdefault，防止預設， adobe.target.trackEvent
description: 使用 [!UICONTROL adobe.target.trackEvent()] 的函式 [!DNL Adobe Target] at.js JavaScript程式庫會引發要求來報告使用者的動作，例如網站上的點按和轉換。
title: 如何使用 [!UICONTROL adobe.target.trackEvent()] 功能？
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 59%

---

# [!UICONTROL adobe.target.trackEvent(options)]

此函數會觸發要求以報告使用者動作，例如點擊和轉換。它不會在回應中傳遞活動。

然後這些事件追蹤 mbox 呼叫可以用來定義活動中的量度。如需詳細資訊，請參閱[成功量度](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)和[追蹤轉換](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)。

以下是 API 詳細資料:

| 機碼 | 類型 | 必要 | 說明 |
|--- |--- |--- |--- |
| mbox | 字串 | 是 | Mbox 名稱<P>**注意**：如果 [!UICONTROL trackEvent()] 使用已在頁面上引發的mbox名稱來引發呼叫，其SDID為 [!UICONTROL trackEvent()] 「 」已重設，且將不同於 [!DNL Target] 頁面上的呼叫。 但是，觸發 [!UICONTROL trackEvent()] 使用不同mbox名稱的呼叫會保留 [!UICONTROL trackEvent()] 呼叫的SDID與 [!UICONTROL 頁面載入請求]/[!UICONTROL triggerView()] 頁面上的呼叫。 |
| selector | 字串 | 無 | 用來尋找 HTML 元素的 CSS 選取器。事件接聽程式將附加至找到的元素。 |
| type | 字串 | 無 | 代表已註冊的事件類型。它可以是 HTML 已知事件，如: 按一下、按下滑鼠等等，也可以是自訂 HTML 事件。 |
| preventDefault | 布林值 | 無 | 指出是否在事件接聽程式回呼中使用 `[!UICONTROL event.preventDefault()]`。預設值設為 false。<P>**注意**：僅限 `[!UICONTROL form[submit]]` 和 `a[click]` 支援。 由於複雜度和要支援的案例數量太大，不支援其他案例。 |
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
