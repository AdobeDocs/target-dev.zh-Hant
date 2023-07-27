---
keywords: 雲端例項、公用尾碼清單、公用尾碼、Cookie、第一方Cookie、azurewebsites.net、cloudapp.net、amazonaws.com、cloudfront.net、herokuapp.com、firebaseapp.com、targetGlobalSettings、cookieDomain、雲端例項5、雲端例項6、雲端例項7、雲端例項8、雲端例項9、公用尾碼清單0、公用尾碼清單1、公用尾碼清單2、公用尾碼清單3、公用尾碼清單4、公用尾碼清單5
description: 探索客戶使用雲端型例項來測試時遇到的（使用解決方案的）問題 [!DNL Adobe Target] 或用於概念證明用途。
title: 我可以使用 [!DNL Target] 使用雲端型例項？
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 46%

---

# 使用雲端型例項搭配 [!DNL Target]

關於客戶使用雲端型例項來測試 [!DNL Adobe Target] 時所面臨問題的資訊。

[!DNL Target] 客戶有時使用雲端型例項搭配 [!DNL Target] 進行測試或簡單的概念證明用途。這些例項可能包含下列網域:

`azurewebsites.net`、`cloudapp.net`、`amazonaws.com`、`cloudfront.net`、`herokuapp.com` 或 `firebaseapp.com`

這些網域和許多其他網域均屬於[公用字尾清單](https://publicsuffix.org/list/public_suffix_list.dat)。

**問題:** 如果您使用這些網域，新式瀏覽器不會儲存 Cookie。

at.js JavaScript資料庫使用Cookie來追蹤使用者，以確保[！DNL [!DNL Target]]永遠呈現一致的體驗。 如果 [!DNL Target] JavaScript程式庫無法儲存Cookie，Target請求已停用。

**解決方案:**&#x200B;如果您想要搭配「公用尾碼清單」上包含的網域來使用雲端式例項，最佳做法是自訂 `cookieDomain` 設定。如需詳細資訊，請參閱 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)。
