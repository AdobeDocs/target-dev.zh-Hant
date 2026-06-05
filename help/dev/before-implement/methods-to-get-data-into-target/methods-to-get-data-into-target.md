---
keywords: 實作，實作，設定，設定，頁面引數， tomcat， url編碼，頁面內設定檔屬性， mbox引數，頁面內設定檔屬性，指令碼設定檔屬性，大量設定檔更新API，單一檔案更新API，客戶屬性，實作5，實作6，實作7，實作8，實作9，實作0，實作1，實作2，實作3，實作4，實作5，資料提供者，資料提供者，資料提供者
description: 將資料匯入 [!DNL Target]  （頁面引數、設定檔屬性、指令碼設定檔屬性、資料提供者、單一和大量設定檔更新API、客戶屬性）。
title: 如何將資料帶入Target？
feature: Implementation
exl-id: a54e306a-ea8e-4d3f-bc5d-b5895b6b9a84
TQID: https://experienceleague.adobe.com/pmlPWRHb9tnrdSFm7s5OZ-RRsJJOxw-ntBY5AeswIcM
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 377
ht-degree: 27%

---

# 方法概觀

關於您可以用來將資料傳入Adobe Target的各種方法的資訊。

可用的方法包括：

| 方法 | 詳細資料 |
| --- | --- |
| [頁面引數](page-parameters.md)<br />（也稱為「mbox引數」） | 頁面參數是直接透過頁面程式碼傳入的名稱/值配對，不儲存在訪客的設定檔中供未來使用。<br />頁面引數對於傳送頁面資料至[!DNL Target]非常有用，這些資料不需要與訪客的設定檔一起儲存以供未來鎖定目標使用。 這些值改用來說明頁面，或使用者在特定頁面上採取的動作。 |
| [頁面內設定檔屬性](in-page-profile-attributes.md)<br />（也稱為「mbox內設定檔屬性） | 頁面內設定檔屬性是指透過頁面程式碼直接傳遞的名稱/值組，這些頁面程式碼儲存在訪客的設定檔中以供日後使用。<br />頁面內設定檔屬性可讓使用者特定的資料儲存在Target的設定檔中，以便日後鎖定目標和細分。 |
| [指令碼輪廓屬性](script-profile-attributes.md) | 指令碼設定檔屬性是在[!DNL Target]解決方案中定義的名稱/值組。 值取決於每次伺服器呼叫在 Target 的伺服器上執行 JavaScript 片段。<br />使用者會撰寫小型程式碼片段，在評估訪客的對象和活動成員資格時，每次mbox呼叫都會執行。 |
| [資料提供者](data-providers.md) | 資料提供者可讓您輕鬆將資料從第三方傳遞至Target。 |
| [大量輪廓更新 API](bulk-profile-update-api.md) | 透過API，傳送.csv檔案至[!DNL Target]並包含許多訪客的訪客設定檔更新。 一次呼叫就能以多個頁面內設定檔屬性來更新每一個訪客設定檔。 |
| [單一輪廓更新 API](single-profile-update-api.md) | 幾乎與大量設定檔更新API相同，但一次會更新一個訪客設定檔，在API呼叫中排成一行，而不是使用.csv檔案。 |
| [客戶屬性](customer-attributes.md) | 客戶屬性可讓您透過 FTP 將訪客設定檔資料上傳至 Experience Cloud。 上傳後，即可在Adobe Analytics和Adobe Target中使用這些資料。 |
