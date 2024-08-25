---
title: 什麼是Adobe Recommendations API？
description: 本指南會逐步引導開發人員使用Adobe Target Recommendations API來設定和管理Recommendations目錄和自訂條件，以及使用傳送API來擷取建議內容。
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 2%

---

# Adobe Recommendations API總覽

與Recommendations相關的API包括[管理員API](../../before-administer/target-api-overview.md)，可讓您：

* 管理建議產品或內容的目錄
* 管理您的Recommendations演演算法和活動

搭配Recommendations使用Target [傳送API](../../implement/delivery-api/overview.md)，您也可以：

* 擷取JSON、HTML或XML物件中的建議，以便這些建議可在網頁、行動裝置、電子郵件、物聯網(IOT)和其他管道中顯示。

## 說明

有關Recommendations API的本指南將引導開發人員使用Recommendations API來設定和管理Recommendations目錄和自訂條件，以及使用傳送API來擷取建議內容的實作練習。 到最後，您將能夠：

* 使用Recommendations API設定和管理實體
* 使用Recommendations API設定和管理自訂條件
* 瞭解如何搭配傳送API使用Recommendations，以在非HTML裝置中使用建議結果

## 客群

本指南適用對象為Target API或Recommendations API的新手開發人員。

## 必備條件 {#prerequisites}

Target管理員API需要[Adobe驗證設定](../configure-authentication.md)。 使用Recommendations API之前，請確定您已設定此專案。

## 資源

請注意下列資源，這些資源是瞭解本指南並成功遵循本指南所必需的：

| 資源 | 詳細資料 |
| --- | --- |
| Postman | 取得您作業系統的[Postman應用程式](https://www.postman.com/downloads/)。 Postman basic可免費建立帳戶。 雖然在一般情況下使用Adobe Target API不需要使用，但Postman可簡化API工作流程，而Adobe Target提供多個Postman集合來協助執行其API並瞭解其運作方式。 本指南的其餘部分假設您具備Postman的工作知識。 如需協助，請參考[Postman檔案](https://learning.getpostman.com/)。 |
| 參考 | 在本指南的其餘部分中假設您熟悉以下資源：<UL><li>[Adobe I/O的Github](https://github.com/adobeio)</li><li>[Target管理員和設定檔API檔案](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API檔案](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
