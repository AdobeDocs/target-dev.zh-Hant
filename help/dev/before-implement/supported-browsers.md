---
keywords: 瀏覽器，先決條件，需求， internet explorer， chrome， firefox， safari， android， surface，瀏覽器0
description: 瞭解哪些網際網路瀏覽器 [!DNL Adobe Target] 支援其介面和內容傳送。
title: ' [!DNL Target] 支援哪些瀏覽器？'
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 26%

---

# 受支援的瀏覽器

[!DNL Adobe Target] 應用程式和內容傳遞已針對廣泛的瀏覽器和裝置進行測試。

如需有關TLS的更多重要資訊，請參閱[TLS （傳輸層安全性）加密變更](tls-transport-layer-security-encryption.md)。

## [!DNL Target] Standard/Premium介面

[!DNL Target]介面支援下列瀏覽器和裝置：

| 裝置類型 | 瀏覽器版本 |
|--- |--- |
| Windows | <ul><li>Microsoft Edge</li><li>Google Chrome （最新，最新 — 1）</li><li>Mozilla Firefox （最新，最新 — 1）</li></ul> |
| Mac | <ul><li>Firefox （最新，最新 — 1）</li><li>Chrome （最新，最新–1）</li></ul> |

## 內容傳遞

內容傳送已對下列瀏覽器和裝置進行測試:

| 裝置類型 | 瀏覽器版本 |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9和10。 在模擬模式中測試。**注意**： at.js 1.3.0 （和更新版本）不再支援IE 9上的內容傳遞。 at.js 2.5.0 （和更新版本）不再支援IE 10、11和所有舊版本上的內容傳送。</li><li>Internet Explorer 11。 **注意**： at.js 2.5.0 （和更新版本）不再支援IE 10、11和所有舊版本上的內容傳遞。</li><li>Microsoft Edge</li><li>Chrome （最新，最新–1）</li><li>Firefox （最新，最新 — 1）</li></ul> |
| Mac | <ul><li>Apple Safari （最新）。 **注意**：如需Safari如何處理第一方和第三方Cookie的詳細資訊，請參閱[Target Cookie](../implement/client-side/atjs/atjs-cookies.md)。</li><li>Firefox （最新，最新 — 1）</li><li>Chrome （最新，最新–1）</li></ul> |
| 行動裝置/平板電腦 | <ul><li>Apple iOS （最新）</li><li>Android 裝置和平板電腦 (Android 4 和更新版本)</li><li>Microsoft Surface (Windows 8.1)</li></ul> |

請注意下列事項：

* 針對at.js實作，[!DNL Target]會在舊版Internet Explorer中並可能在以上所列瀏覽器的舊版本中顯示預設內容。
* Internet Explorer會將所有未知的元素（例如自訂元素）視為相同的元素型別。 因此，傳送不適用於自訂元素。
* [!DNL Target]會在以上未列出的瀏覽器以及使用[怪異模式](https://en.wikipedia.org/wiki/Quirks_mode)的瀏覽器中顯示預設內容。 at.js 需要可呈現標準模式的 doctype，例如: `<!DOCTYPE html>`。
* [!DNL Adobe]傳遞基礎結構已受保護，2018年9月12日之後「不」支援TLS 1.0裝置和瀏覽器。 請參閱 [TLS (傳輸層安全性) 加密變更](../before-implement/tls-transport-layer-security-encryption.md)，瞭解這項變更帶來的整體影響。
