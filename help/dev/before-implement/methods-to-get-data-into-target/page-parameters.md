---
keywords: 實作，實施，設定，設定，頁面引數
description: 使用頁面引數將資料匯入 [!DNL Target] 。
title: 如何使用頁面引數將資料匯入 [!DNL Target] ？
feature: Implementation
exl-id: 9bb7157e-a938-4150-8a15-c9bf0a0e2296
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 32%

---

# 頁面參數

頁面引數（也稱為「mbox引數」）是直接透過頁面程式碼傳入的名稱/值組，不會儲存在訪客的設定檔中以供日後使用。

頁面引數可用來將不需要與訪客的設定檔儲存以供未來鎖定目標使用的頁面資料傳送至[!DNL Adobe Target]。 這些值改用來說明頁面，或使用者在特定頁面上採取的動作。

## 格式

頁面引數會透過伺服器呼叫以字串名稱/值配對的形式傳遞至[!DNL Target]。 參數名稱和值可自訂 (但有一些「保留名稱」是特定用途)。

以下是頁面引數的一些範例

* `page=productPage`

* `categoryId=homeLoans`

## 範例使用案例

* **產品頁面**：傳送已檢視之特定產品的相關資訊(此方法為Recommendations的運作方式)
* **訂單詳細資料**：傳送訂單識別碼、orderTotal等供收集訂單
* **類別相關性**：將類別檢視資訊傳送至[!DNL Target]，以瞭解使用者與特定網站類別的相關性
* **第三方資料**: 傳送來自第三方資料來源的資訊，例如，天氣鎖定目標提供者、帳戶資料 (例如 DemandBase)、人口統計資料 (例如 Experian) 及其他。

## 方法的優點

資料會即時傳送至[!DNL Target]，並可在相同伺服器呼叫資料所傳入的資料時使用。

## 注意事項

* 需要頁面程式碼更新 (直接或透過標記管理系統)。
* 如果資料必須在後續的頁面/伺服器呼叫上用於定位，則必須將其轉譯為設定檔指令碼。
* 查詢字串只能包含符合[網際網路工程任務小組(IETF)標準](https://www.ietf.org/rfc/rfc3986.txt)的字元。

  除了在IETF網站上提到的字元外，[!DNL Target]還允許在查詢字串中包含下列字元：

  ```< > # % " { } | \ ^ [ ] ` ``` {line-numbers=&quot;true&quot;}

  除此之外的字元都必須經過 URL 編碼。此標準指定了下列格式( [https://www.ietf.org/rfc/rfc1738.txt](https://www.ietf.org/rfc/rfc1738.txt) )，如下圖所示：

  ![替代影像](assets/ietf1.png)

  或者，為簡單起見，下列為完整清單:

  ![替代影像](assets/ietf2.png)

## 程式碼範例

targetPageParamsAll (將參數附加至頁面上的所有 mbox 呼叫):

`function targetPageParamsAll() { return "param1=value1&param2=value2&p3=hello%20world";`

targetPageParams (將參數附加至頁面上的全域 mbox):

`function targetPageParams() { return "param1=value1&param2=value2&p3=hello%20world";`

## 相關資訊的連結

建議: [根據頁面類型實作](https://experienceleague.adobe.com/docs/target/using/recommendations/plan-implement.html)

訂單確認: [追蹤轉換](../../implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)

類別相關性: [類別相關性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/category-affinity.html)
