---
title: Adobe Target Delivery API考量事項和已知限制
description: 使用時，應考慮哪些注意事項和已知限制 [!UICONTROL Adobe Target傳送API]？
keywords: 傳送api
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 49acf92bbe06dbcee36fef2b7394acd7ce37baad
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 6%

---

# 考量事項和已知限制

* 「 」沒有驗證 [!DNL Target] 傳送API。
* 本 API 不會處理 Cookie 或重新導向呼叫。
* HTTP/1.1和HTTP/2標頭名稱不區分大小寫，但HTTP/2會強制使用小寫標頭名稱。 如需詳細資訊，請參閱 [超文字傳輸通訊協定第2版(HTTP/2)檔案](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  如果您使用引導訪客通過新負載平衡器基礎結構的端點，其連線會自動升級為HTTP/2。 此升級程式會將請求標頭轉換為小寫標頭，以免被視為格式錯誤。

  如果客戶的程式庫設為尋找區分大小寫（尤其是非小寫）的請求/回應標頭，則此問題可能會成為客戶的問題。
