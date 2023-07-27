---
keywords: 電子郵件、adbox、電子郵件影像adbox
description: 瞭解如何使用 [!DNL Adobe Target] 以動態測試電子郵件中的影像，甚至在某人開啟電子郵件時即時變更這些影像。
title: 如何測試電子郵件影像Adbox？
feature: Implement Email
exl-id: 4512741a-567f-41bb-9721-3e1c4f5302e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 84%

---

# 測試電子郵件影像 Adbox

動態測試電子郵件中的影像，甚至在某人開啟電子郵件時迅速變更那些影像。

您可以在電子郵件中使用重新導向程式，以追蹤點選並動態控制訪客會抵達的登陸頁面。

您可以透過使用修改版的 adbox，來完成電子郵件影像測試。由於電子郵件用戶端不允許設定 Cookie，所以每封電子郵件都必須產生唯一的識別碼。這個代碼會附加至 adbox URL 以及電子郵件中使用的任何重新導向程式，以追蹤來自電子郵件的點選。

>[!NOTE]
>
>雖然 [!DNL Target] 技術上可在開啟時最佳化影像，每個電子郵件使用者端處理快取的方式不同，因此不一定能成功。 不論使用的電子郵件服務提供者為何，使用者用來開啟電子郵件的電子郵件用戶端 (Gmail 應用程式、Outlook、Hotmail 等) 會決定實際上是否能在開啟時擷取影像。舉例來說，Gmail 會快取大部分內容，因此使用者看到的影像是根據 Gmail 何時選擇呼叫並快取影像。

**電子郵件影像 adbox 的簡單程式碼:**

```
<img src="https://{clientcode}.tt.omtrdc.net/m2/​{clientcode}/ubox/​image?
mbox={email_header}&
mboxDefault=​{http%3A%2F%2Fwww.domain.com%2Fheader.jpg}&
mboxXDomain=disabled&
mboxSession={123456}&
mboxPC={123456}" border=:"0"/>
```

其中以下的值是您專屬的:

| 值 | 說明 |
|--- |--- |
| clientcode | 公司的用戶端代碼。在at.js中找出列為「 」的此代碼 `clientCode='yourclientcode'`. 全部都是小寫，不包含特殊字元。 |
| 影像 | 選件類型。對於影像廣告一律為「image」，而對於重新導向程式則為「page」。 |
| email_header | adbox 的名稱。 |
| `mboxDefault=http%3A%2F%2Fwww.domain.com%2Fheader.jpg` | 必要。將 URL 取代為適當的 Adbox 預設內容。此網址必須是絕對參照，並且必須經過 URL 編碼。 |
| `mboxXDomain=disabled` | 告訴 [!DNL Target] 不要嘗試設定 Cookie。 |
| `mboxSession=123456`和`mboxPC=123456` | [!DNL Target] 需要的兩個值，用來合併此使用者的設定檔與您的網站的現有設定檔。123456 是每封電子郵件都會產生的唯一識別碼。請動態地將此值插入每個 adbox 與重新導向程式的 URL 中。對於傳送給每位使用者的每封電子郵件，此代碼都必須是專屬的代碼。如果某封每週電子郵件要傳送給 1,000 個人，就必須產生 1,000 個專屬的 ID。<br />每封電子郵件的唯一識別碼都必須在每個 adbox 與重新導向程式的 URL 中，指定給 mboxSession 和 mboxPC。此識別碼的建議格式為「時間戳記-NNNN」，其中 NNNNN 為隨機的 5 位數字，但任何數字與字母的組合均為有效格式。某些大量電子郵件服務以及所有的程式語言都能夠產生這種唯一的識別碼。 |
