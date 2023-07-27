---
keywords: 實作，實施，設定，設定，頁面引數
description: 將資料匯入 [!DNL Target] 使用頁面內設定檔屬性。
title: 如何將資料帶入 [!DNL Target] 使用頁面內設定檔屬性？
feature: Implementation
exl-id: c19fd746-21a2-4eb5-8c2a-c24806e09324
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 45%

---

# 頁面中設定檔屬性

中的頁面內設定檔屬性 [!DNL Adobe Target] （也稱為「in-mbox profile attributes」）是直接透過頁面程式碼傳遞的名稱/值組，這些名稱/值組會儲存在訪客的設定檔中以供日後使用。

頁面內設定檔屬性允許將使用者特定的資料儲存在 Target 的設定檔中，供稍後鎖定目標和分割。

## 格式

頁面內設定檔屬性會傳遞至 [!DNL Target] 透過伺服器呼叫，將字串名稱/值配對加上前置詞「profile」。 屬性名稱之前。

屬性名稱和值可自訂 (但有一些「保留名稱」是特定用途)。

以下是頁面內設定檔屬性的部分範例：

* `profile.membershipLevel=silver`
* `profile.visitCount=3`

## 範例使用案例

* **登入資訊**[!DNL Target]: 根據使用者的登入將非 PII (個人識別資訊) 資料與 共用。此資料可能是成員資格狀態、訂單歷史記錄或更多。
* **商店資訊**: 追蹤哪一家商店是此使用者偏好的位置。
* **先前的互動**: 追蹤使用者先前在網站上完成的動作，以提供未來個人化的相關資訊。

## 方法的優點

資料傳送到 [!DNL Target] 和可供即時用於資料傳入的相同伺服器呼叫。

## 注意事項

需要頁面程式碼更新 (直接或透過標記管理系統)。

屬性和值在伺服器呼叫中可見，所以訪客可以看到值。如果共用資訊（例如信用範圍或其他潛在的私人資訊），則此方法可能不是最佳方法。

## 程式碼範例

targetPageParamsAll (將屬性附加至頁面上的所有 mbox 呼叫):

`function targetPageParamsAll() { return "profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

targetPageParams (將屬性附加至頁面上的全域 mbox):

`function targetPageParams() { return profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

mboxCreate 程式碼中的屬性:

`<div class="mboxDefault"> default content to replace by offer </div> <script> mboxCreate('mboxName','profile.param1=value1','profile.param2=value2'); </script>`

## 相關資訊的連結

[設定檔屬性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html)

[訪客資料](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html)
