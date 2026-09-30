---
title: 什麼是Adobe Recommendations API？
description: 本指南會使用Adobe Target Recommendations API來設定和管理Recommendations目錄和自訂條件，以及使用傳送API來擷取Recommendations內容，向開發人員逐步說明實作步驟。
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail:
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
TQID: 'https://experienceleague.adobe.com/-bWsxWNZK7LXp0VvKZmsZc68jXcit57v7Wki9hR3wH4'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: a19e8738-9679-599a-b83b-5f2f15f8e4d6
    internal-label: APIs/SDKs
  - id: dfc8a233-f2b5-4811-bf63-b4262aebc5a5
    internal-label: Administration and configuration
  - id: f69bc5f1-ebdb-4306-a281-f2e77daf734c
    internal-label: Activities and tests
subfeature_v2:
  - id: ed58f4a1-16eb-4c8c-b505-be9da766a9ec
    internal-label: Recommendations
  - id: fc9c2184-9102-403f-bd6c-0055021e4bea
    internal-label: Overview
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
    internal-label: Reporting
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 3%
---
# Adobe Recommendations API總覽

與Recommendations相關的API包括[管理員API](../../before-administer/target-api-overview.md)，可讓您：

* 管理建議產品或內容的目錄
* 管理您的Recommendations演演算法和活動

搭配使用Target [傳送API](../../implement/delivery-api/overview.md)與Recommendations，您也可以：

* 擷取JSON、HTML或XML物件中的建議，以便這些建議可在網頁、行動裝置、電子郵件、物聯網(IOT)和其他管道中顯示。

## 說明

有關Recommendations API的本指南將引導開發人員使用Recommendations API來設定和管理Recommendations目錄和自訂條件，以及使用傳送API來擷取建議內容的實作練習。 到最後，您將能夠：

* 使用Recommendations API設定和管理實體
* 使用Recommendations API設定和管理自訂條件
* 瞭解如何搭配Delivery API使用Recommendations，以便在非HTML裝置中使用建議結果

## 客群

本指南適用對象為Target API或Recommendations API的新手開發人員。

## 先決條件 {#prerequisites}

Target管理員API需要[Adobe驗證設定](../configure-authentication.md)。 使用Recommendations API之前，請確定您已設定此專案。

## 資源

請注意下列資源，這些資源是瞭解本指南並成功遵循本指南所必需的：

| 資源 | 詳細資料 |
| --- | --- |
| Postman | 取得您作業系統的[Postman應用程式](https://www.postman.com/downloads/)。 Postman basic可免費建立帳戶。 雖然在一般情況下使用Adobe Target API不需要使用，但Postman可簡化API工作流程，而Adobe Target提供多個Postman集合來協助執行其API並瞭解其運作方式。 本指南的其餘部分假設您具備Postman的工作知識。 如需協助，請參考[Postman檔案](https://learning.getpostman.com/)。 |
| 參考 | 在本指南的其餘部分中假設您熟悉以下資源：<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Target管理員和設定檔API檔案](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API檔案](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
