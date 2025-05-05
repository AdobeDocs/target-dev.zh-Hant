---
keywords: 內容安全性原則， csp， at.js，白名單，允許清單，忽隱忽現，預先隱藏，預先隱藏，內容安全性原則， iFrame， iframe
description: 瞭解在使用 [!DNL Adobe Target]時應該新增的內容安全性原則(CSP)指示。
title: ' [!DNL Target] 如何處理內容安全性原則 (CSP)？'
feature: Privacy & Security
exl-id: ec6942e5-36d8-4f88-b3d6-47f9eaca03a8
source-git-commit: c43c79b29768694eac534e22047b5ee6a3d0ccd5
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 28%

---

# 內容安全性原則 (CSP) 指示

如果您正在使用[內容安全性原則](https://zh.wikipedia.org/wiki/Content_Security_Policy) (CSP)進行[!DNL Adobe Target]實作，您應該在使用[at.js 2.1或更新版本](../../implement/client-side/atjs/target-atjs-versions.md)時新增下列CSP指示：

* `connect-src` 並將 `*.tt.omtrdc.net` 加入允許清單。 允許將網路要求傳送到 [!DNL Target] 邊緣所需。
* `style-src unsafe-inline`。 預先隱藏和閃現控制所需。
* `script-src unsafe-inline`。允許執行可能是 HTML 選件之一部分的 JavaScript 所需。

## 常見問題集 (FAQ)

查詢以下關於內容安全性原則的常見問題：

### 跨原始資源共用 (CORS) 和 Flash 跨網域原則會出現安全問題嗎？

實施 CORS 原則的推薦方法是允許存取限可信任的來源；這類來源規定要通過受信任網域的允許清單來進行。Flash 跨網域原則也是同樣的道理。 有些[!DNL Target]客戶擔心在Target網域中使用萬用字元。 問題是如果使用者已登入應用程式，並瀏覽原則允許的網域，則該網域上執行的任何惡意內容都可能從應用程式中擷取敏感內容，並在已登入使用者的安全上下文中執行操作。 這種情況通常稱為跨網站請求偽造(CSRF)。

但是，在[!DNL Target]實作中，這些原則應該不表示有安全問題。

「adob&#x200B;&#x200B;e.tt.omtrdc.net」是 Adobe 擁有的網域。 [!DNL Adobe Target] 是一種測試和個人化工具，[!DNL Target] 應可從任何地方接收和處理請求，而無需任何身份驗證。 這些請求包含用來進行 A/B 測試、建議或內容個人化的機碼/值組。

Adobe不會在「adobe.tt.omtrdc.net」指向的[!DNL Adobe Target]邊緣伺服器上儲存個人識別資訊(PII)或其他敏感資訊。

[!DNL Target] 應可透過 JavaScript 呼叫從任何網域存取。 允許這項存取的唯一方法是套用「Access-Control-Allow-Origin」搭配萬用字元。

### 我要如何允許或防止我的網站被內嵌為iFrame在外部網域下？

若要允許[視覺化體驗撰寫器](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=zh-Hant){target=_blank} (VEC)將您的網站內嵌於iFrame中，必須在您的網頁伺服器設定上變更CSP （若已設定）。 必須將[!DNL Adobe]個網域列入白名單並進行設定。

基於安全考量，您可能會想要防止網站被內嵌為iFrame在外部網域之下。

以下章節說明如何允許或防止VEC將您的網站內嵌於iFrame。

#### 允許VEC將您的網站內嵌於iFrame中

讓VEC將您的網站內嵌於iFrame的最簡單解決方案是允許`*.adobe.com` （最廣泛的萬用字元）。

例如：

`Content-Security-Policy: frame-ancestors 'self' *.adobe.com`

如下圖所示（按一下以放大）：


![CSP具有最廣泛的萬用字元](/help/dev/before-implement/privacy/assets/csp-adobe.png){width="600" zoomable="yes"}

您可能只想允許實際的[!DNL Adobe]服務。 可使用`*.experiencecloud.adobe.com + https://experiencecloud.adobe.com`達成此情境。

例如：

`Content-Security-Policy: frame-ancestors 'self' https://*.experiencecloud.adobe.com https://experiencecloud.adobe.com https://experience.adobe.com`

如下圖所示（按一下以放大）：

![CSP與ExperienceCloud限定範圍](/help/dev/before-implement/privacy/assets/csp-experiencecloud.png){width="600" zoomable="yes"}

使用`https://<Client Code>.experiencecloud.adobe.com https://experience.adobe.com`可以存取公司帳戶的最嚴格限制，其中`<Client Code>`代表您特定的使用者端代碼。

例如：

`Content-Security-Policy: frame-ancestors 'self'  https://ags118.experiencecloud.adobe.com https://experience.adobe.com`

如下圖所示（按一下以放大）：

![CSP的clientcode範圍是](/help/dev/before-implement/privacy/assets/csp-clientcode.png){width="600" zoomable="yes"}

>[!NOTE]
>
>如果您已實施[啟動/標籤](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)，則也必須將其解除鎖定。
>
>例如：
>
> `Content-Security-Policy: frame-ancestors 'self' *.adobe.com *.assets.adobedtm.com;`

#### 防止VEC將您的網站內嵌於iFrame中

若要防止VEC將您的網站內嵌於iFrame中，您可以限製為「本身」。

例如：

`Content-Security-Policy: frame-ancestors 'self'`

如下圖所示（按一下以放大）：

![CSP錯誤](/help/dev/before-implement/privacy/assets/csp-error.png){width="600" zoomable="yes"}

會顯示下列錯誤訊息：

`Refused to frame 'https://kuehl.local/' because an ancestor violates the following Content Security Policy directive: "frame-ancestors 'self'".`

