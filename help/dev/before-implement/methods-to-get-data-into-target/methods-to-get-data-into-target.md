---
keywords: 實作，實作，設定，設定，頁面引數， tomcat， url編碼，頁面內設定檔屬性， mbox引數，頁面內設定檔屬性，指令碼設定檔屬性，大量設定檔更新API，單一檔案更新API，客戶屬性，實作5，實作6，實作7，實作8，實作9，實作0，實作1，實作2，實作3，實作4，實作5，資料提供者，資料提供者，資料提供者
description: 將資料匯入 [!DNL Target] （頁面引數、設定檔屬性、指令碼設定檔屬性、資料提供者、單一和大量設定檔更新API、客戶屬性）。
title: 如何將資料帶入Target？
feature: Implementation
exl-id: a54e306a-ea8e-4d3f-bc5d-b5895b6b9a84
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 44%

---

# 方法概覽

關於您可以用來將資料傳入Adobe Target的各種方法的資訊。

可用的方法包括：

| 方法 | 詳細資料 |
| --- | --- |
| [頁面引數](page-parameters.md)<br />（又稱為「mbox引數」） | 頁面參數是直接透過頁面程式碼傳入的名稱/值配對，不儲存在訪客的設定檔中供未來使用。<br />頁面引數對於傳送頁面資料至很有用 [!DNL Target] 這些資料不需要儲存在訪客的設定檔中，以供日後鎖定目標使用。 這些值改用來說明頁面，或使用者在特定頁面上採取的動作。 |
| [頁面中設定檔屬性](in-page-profile-attributes.md)<br />（也稱為「in-mbox」設定檔屬性） | 頁面內設定檔參數是直接透過頁面程式碼傳遞的名稱/值配對，儲存在訪客的設定檔中供未來使用。<br />頁面內設定檔屬性允許將使用者特定的資料儲存在 Target 的設定檔中，供稍後鎖定目標和分割。 |
| [指令碼設定檔屬性](script-profile-attributes.md) | 指令碼設定檔屬性是在 [!DNL Target] 解決方案。 值取決於每次伺服器呼叫在 Target 的伺服器上執行 JavaScript 片段。<br />使用者撰寫較小的程式碼片段，在每次 mbox 呼叫時執行，以及在評估訪客的對象和活動成員資格之前執行。 |
| [資料提供者](data-providers.md) | 資料提供者可讓您輕鬆將資料從第三方傳遞至Target。 |
| [大量設定檔更新 API](bulk-profile-update-api.md) | 透過API傳送.csv檔案至 [!DNL Target] 更新許多訪客的訪客設定檔。 一次呼叫就能以多個頁面內設定檔屬性來更新每一個訪客設定檔。 |
| [單一設定檔更新 API](single-profile-update-api.md) | 幾乎與大量設定檔更新API相同，但一次會更新一個訪客設定檔，在API呼叫中排成一行，而不是使用.csv檔案。 |
| [客戶屬性](customer-attributes.md) | 客戶屬性可讓您透過 FTP 將訪客設定檔資料上傳至 Experience Cloud。上傳後，即可在Adobe Analytics和Adobe Target中使用這些資料。 |
