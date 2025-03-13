---
keywords: 瀏覽器，先決條件，需求， internet explorer， chrome， firefox， safari， android， surface，瀏覽器0
description: 瞭解哪些網際網路瀏覽器 [!DNL Adobe Target] 支援其介面和內容傳送。
title: ' [!DNL Target] 支援哪些瀏覽器？'
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: 1b6dcb24d677b758ed1daf85dc0a7e9e5b42680d
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 20%

---

# 受支援的瀏覽器

[!DNL Adobe Target] 應用程式和內容傳遞已針對廣泛的瀏覽器和裝置進行測試。

如需有關TLS的更多重要資訊，請參閱[TLS （傳輸層安全性）加密變更](tls-transport-layer-security-encryption.md)。

## [!DNL Target] Standard/Premium介面

[!DNL Target]介面支援下列瀏覽器和裝置：

>[!NOTE]
>
>Target支援所列各瀏覽器的最新版本，且最新版本會減去1。


| 裝置類型 | 瀏覽器版本 |
|--- |--- |
| [!DNL Windows] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |
| [!DNL Mac] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |

## 視覺化編輯需求

若要在[!UICONTROL Visual Experience Composer] (VEC)中以可靠的方式開啟、製作及預覽您的網頁，您必須在網頁瀏覽器上安裝[Adobe Experience Cloud Visual Editing Helper瀏覽器擴充功能](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}，或使用[!UICONTROL Enhanced Experience Composer (EEC)]。

>[!NOTE]
>
>[!DNL Google Chrome]和[!DNL Microsoft Edge]目前是唯一支援[!DNL Adobe Target]網頁視覺化編輯的瀏覽器。


## 內容傳遞

內容傳送已對下列瀏覽器和裝置進行測試:

| 裝置類型 | 瀏覽器版本 |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9和10。 在模擬模式中測試。**注意**： at.js 1.3.0 （和更新版本）不再支援IE 9上的內容傳遞。 at.js 2.5.0 （和更新版本）不再支援IE 10、11和所有舊版本上的內容傳送。</li><li>Internet Explorer 11。 **注意**： at.js 2.5.0 （和更新版本）不再支援IE 10、11和所有舊版本上的內容傳遞。</li><li>Microsoft Edge</li><li>Chrome （最新，最新–1）</li><li>Firefox （最新，最新 — 1）</li></ul> |
| Mac | <ul><li>Apple Safari （最新）。 **注意**：如需Safari如何處理第一方和第三方Cookie的詳細資訊，請參閱[Target Cookie](../implement/client-side/atjs/atjs-cookies.md)。</li><li>Firefox （最新，最新 — 1）</li><li>Chrome （最新，最新–1）</li></ul> |
| 行動裝置/平板電腦 | <ul><li>Apple iOS （最新）</li><li>Android 裝置和平板電腦 (Android 4 和更新版本)</li><li>Microsoft Surface (Windows 8.1)</li></ul> |

請注意下列事項：

* [!DNL Adobe Experience Platform Web SDK]的設計可在最新版本的[!DNL Google Chrome]、[!DNL Safari]、[!DNL Firefox]和[!DNL Microsoft Edge Chromium]中以最佳方式運作。 您可能無法在這些舊版的瀏覽器或已棄用的瀏覽器（例如[!DNL Internet Explorer]）上使用某些功能。
* 針對at.js實作，[!DNL Target]會在舊版Internet Explorer中並可能在以上所列瀏覽器的舊版本中顯示預設內容。
* Internet Explorer會將所有未知的元素（例如自訂元素）視為相同的元素型別。 因此，傳送不適用於自訂元素。
* [!DNL Target]會在以上未列出的瀏覽器以及使用[怪異模式](https://en.wikipedia.org/wiki/Quirks_mode)的瀏覽器中顯示預設內容。 at.js 需要可呈現標準模式的 doctype，例如: `<!DOCTYPE html>`。
* [!DNL Adobe]傳遞基礎結構已受保護，2018年9月12日之後「不」支援TLS 1.0裝置和瀏覽器。 請參閱 [TLS (傳輸層安全性) 加密變更](../before-implement/tls-transport-layer-security-encryption.md)，瞭解這項變更帶來的整體影響。
