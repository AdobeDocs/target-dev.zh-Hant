---
title: Adobe Target Delivery API考量事項和已知限制
description: 使用[!UICONTROL Adobe Target Delivery API]時，應考慮哪些考量和已知限制？
keywords: 傳送api
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 413b16ed0b098de6914558fa29b9ca59aaba958e
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 3%

---

# 考量事項和已知限制

下列資訊列出使用[!DNL Adobe Target] [!DNL Delivery API]時的考量事項和已知限制。

* [!DNL Target]傳遞API沒有驗證。
* 本 API 不會處理 Cookie 或重新導向呼叫。
* HTTP/1.1和HTTP/2標頭名稱不區分大小寫，但HTTP/2會強制使用小寫標頭名稱。 如需詳細資訊，請參閱[Hypertext Transfer Protocol Version 2 (HTTP/2)檔案](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}。

  如果您使用引導訪客通過新負載平衡器基礎結構的端點，其連線會自動升級為HTTP/2。 此升級程式會將請求標頭轉換為小寫標頭，以免被視為格式錯誤。

  如果客戶的程式庫設為尋找區分大小寫（尤其是非小寫）的請求/回應標頭，則此問題可能會成為客戶的問題。

* 透過[!DNL Recommendations]更新您的[!UICONTROL Catalog] [!DNL Delivery API]時請小心。 [!DNL Delivery API]是公開的，因此請避免使用它來填入建議目錄中的可點按專案。 這樣做可能會引入失效的內容並汙染您的目錄。

  **最佳實務**：

  僅使用[!DNL Delivery API]來更新符合下列條件的目錄屬性：
   * 經常變更（例如價格、庫存量）。
   * 遵循可在您的網站上輕鬆驗證的預定義格式。
   * 請勿將其用於新增或修改可點按專案或其他未經驗證的內容。
   * 如有需要，您可以透過傳送API請求客戶支援以停用目錄更新。

  如需詳細資訊，請參閱[[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank}檔案。
