---
keywords: 實作，實施，設定，設定，頁面引數
description: 使用頁面內設定檔屬性將資料匯入 [!DNL Target] 。
title: 如何使用頁面內設定檔屬性將資料匯入 [!DNL Target] ？
feature: Implementation
exl-id: c19fd746-21a2-4eb5-8c2a-c24806e09324
TQID: https://experienceleague.adobe.com/jXWNNl7HfrR03tEoMz7KApX3onc1Zc44IAuXy4QF2tU
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 40%

---

# 頁面中輪廓屬性

[!DNL Adobe Target]中的頁面內設定檔屬性（也稱為「mbox內設定檔屬性」）是直接透過頁面程式碼傳遞的名稱/值組，這些名稱/值組會儲存在訪客的設定檔中以供日後使用。

頁面內設定檔屬性允許將使用者特定的資料儲存在 Target 的設定檔中，供稍後鎖定目標和分割。

## 格式

頁面內設定檔屬性會透過伺服器呼叫以字串名稱/值組的形式傳遞至[!DNL Target]，前置詞為「profile」。 屬性名稱之前。

屬性名稱和值可自訂 (但有一些「保留名稱」是特定用途)。

以下是頁面內設定檔屬性的部分範例：

* `profile.membershipLevel=silver`
* `profile.visitCount=3`

## 範例使用案例

* **登入資訊**：根據使用者的登入將非PII （個人識別資訊）資料分享給[!DNL Target]。 此資料可能是成員資格狀態、訂單歷史記錄或更多。
* **商店資訊**: 追蹤哪一家商店是此使用者偏好的位置。
* **先前的互動**: 追蹤使用者先前在網站上完成的動作，以提供未來個人化的相關資訊。

## 方法的優點

資料會即時傳送至[!DNL Target]，並可用於資料傳入的相同伺服器呼叫。

## 注意事項

需要頁面程式碼更新 (直接或透過標記管理系統)。

屬性和值在伺服器呼叫中可見，所以訪客可以看到值。 如果共用資訊（例如信用範圍或其他潛在的私人資訊），則此方法可能不是最佳方法。

## 程式碼範例

targetPageParamsAll (將屬性附加至頁面上的所有 mbox 呼叫):

`function targetPageParamsAll() { return "profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

targetPageParams (將屬性附加至頁面上的全域 mbox):

`function targetPageParams() { return profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

mboxCreate 程式碼中的屬性:

`<div class="mboxDefault"> default content to replace by offer </div> <script> mboxCreate('mboxName','profile.param1=value1','profile.param2=value2'); </script>`

## 相關資訊的連結

[描述檔屬性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html)

[訪客輪廓](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html)
