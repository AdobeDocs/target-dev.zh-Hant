---
keywords: 實作，實施，設定，設定，資料提供者
description: 使用資料提供者將資料匯入 [!DNL Target] 。
title: 如何使用資料提供者將資料帶入 [!DNL Target] ？
feature: Implementation
exl-id: 9971bd96-f736-4965-afe2-b4901c12d006
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 55%

---

# 資料提供者

資料提供者是一項功能，可讓您輕鬆將資料從第三方傳遞至[!DNL Adobe Target]。

>[!NOTE]
>
>資料提供者需使用at.js 1.3或更新版本。

## 格式

`window.targetGlobalSettings.dataProviders` 設定是資料提供者的陣列。

如需各資料提供者結構的詳細資訊，請參閱[資料提供者](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)一節。

## 範例使用案例

從第三方收集氣象服務、DMP 甚至您自己的網頁服務等資料。接著，您就能使用此資料來建立對象、鎖定內容及擴充訪客設定檔。

## 方法的優點

此設定可讓客戶收集來自協力廠商資料提供者（例如Demandbase、BlueKai和自訂服務）的資料，並在全域mbox要求中以mbox引數的形式傳遞資料至[!DNL Target]。

它透過非同步和同步要求，以支援來自多個提供者的資料收集。

使用此方法可讓您方便管理預設頁面內容的忽隱忽現情形，同時對每個提供者包含獨立的逾時計算，以限制對頁面效能的影響

## 注意事項

如果新增至`window.targetGlobalSettings.dataProviders`的資料提供者非同步，則會並行執行。 訪客API要求與新增至`window.targetGlobalSettings.dataProviders`的函式並行執行，以允許最低的等待時間。

at.js不會嘗試快取資料。 如果資料提供者擷取資料一次，則資料提供者應該確定已將該資料快取，並且當叫用該提供者函數時，可做為第二個叫用的快取資料。

## 程式碼範例

[資料提供者](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)中提供許多範例。

## 相關資訊的連結

文件: [資料提供者](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)

## 培訓影片：

* [使用 Adobe Target 中的資料提供者](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html?lang=zh-Hant)
* [實作 Adobe Target 中的資料提供者](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html?lang=zh-Hant)
