---
title: Adobe Target Delivery API考量事項和已知限制
description: 使用[!UICONTROL Adobe Target Delivery API]時，應考慮哪些考量和已知限制？
keywords: 傳送api
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/LCgGZONQpYfw6JxNCNc2Iu13Mft8Zfx-3Uxvm2EeUVk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 154
ht-degree: 5%

---

# 考量事項和已知限制

下列資訊列出使用[!DNL Adobe Target] [!DNL Delivery API]時的考量事項和已知限制。

* [!DNL Target]傳遞API沒有驗證。
* 本 API 不會處理 Cookie 或重新導向呼叫。
* HTTP/1.1和HTTP/2標頭名稱不區分大小寫，但HTTP/2會強制使用小寫標頭名稱。 如需詳細資訊，請參閱[Hypertext Transfer Protocol Version 2 (HTTP/2)檔案](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}。

  如果您使用引導訪客通過新負載平衡器基礎結構的端點，其連線會自動升級為HTTP/2。 此升級程式會將請求標頭轉換為小寫標頭，以免被視為格式錯誤。

  如果客戶的程式庫設為尋找區分大小寫（尤其是非小寫）的請求/回應標頭，則此問題可能會成為客戶的問題。
