---
keywords: api， adobe i/o， classic， adobe developer console
description: 瞭解如何從 [!DNL Adobe Target Classic] API轉換到 [!DNL Adobe Developer Console]上的 [!DNL Target] API。
title: 如何從 [!DNL Adobe Developer Console]上的 [!DNL Target Classic] API轉換成 [!DNL Target] API？
feature: APIs/SDKs
exl-id: b84e3767-89ad-4e2d-9bb4-7e31bffbc285
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 38%

---

# 從[!DNL Adobe Developer Console]上的[!DNL Target Classic] API轉變為[!DNL Target] API

可協助您從[!DNL Target Classic] API轉換至[[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home)上[!DNL Target] API的資訊。

隨著[!DNL Adobe Target Classic]的停用，連線至您[!DNL Target Classic]帳戶的API也已無法使用。 本文會協助您將以[!DNL Target Classic] API為基礎的整合轉換至由[[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home)支援的[!DNL Target] API。

如需[!DNL Target] API的詳細資訊，請參閱[[!DNL Target] API](/help/dev/before-administer/target-api-overview.md)。 如需[!DNL Target] SDK的詳細資訊，請參閱[[!DNL Target] 伺服器端實作](/help/dev/implement/server-side/server-side-overview.md)

## 術語

| 術語 | 說明 |
|--- |--- |
| 傳統API | 連結至您[!DNL Target Classic]帳戶的API。 這些 API 呼叫是根據使用者名稱和密碼式驗證，並使用主機名稱 `testandtarget.omniture.com`。如果您的API呼叫在要求URL中包含使用者名稱和密碼，您必須轉換至[!DNL Adobe Developer Console] API。 |
| [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) | [!DNL Adobe Developer Console]是[!DNL Target] API的閘道。 這些API已連線至您的[!DNL Target Standard/Premium]帳戶。 [!DNL Adobe Developer Console]上的[!DNL Target] API使用[JWT型驗證](../../before-administer/configure-authentication.md)，這是安全企業API的產業標準。 |

## 時間表

[!DNL Target Classic]解除委任時，下列API已解除委任：

| 日期 | 詳細資料 |
|--- |--- |
| 2017 年 10 月 17 日 | 已停止支援執行寫入作業的所有 API 方法 (`saveCampaign`、`copyCampaign`、`saveHTMLOfferContent` 以及 `setCampaignState`)。<P>這是所有[!DNL Target Classic]使用者帳戶設為唯讀狀態的相同日期。 |
| 2017 年 11 月 14 日 | 其餘 API 已停用。這是[!DNL Target Classic]使用者介面解除委任的日期 |

[!DNL Recommendations Classic]個API未受此時間表影響。

## 對等方法

下表列出等同於傳統API方法的[!DNL Adobe Developer Console] API方法。 [!DNL Adobe Developer Console] API傳回JSON，而Classic API傳回XML。

[!DNL Adobe Developer Console] API方法已連結至API檔案網站中的對應區段。 每一個 API 方法都有範例。您也可以使用[!DNL Target]管理員API Postman集合，此集合包含[!DNL Adobe Developer Console]上所有AdobeAPI方法的範例API呼叫。

| 分組 | 傳統API方法 | [!DNL Adobe Developer Console] API方法 | 附註 |
|--- |--- |--- |--- |
| 行銷活動/活動 | 行銷活動建立 | [建立 AB 活動](https://developers.adobetarget.com/api/#create-ab-activity)<P>[建立 XT 活動](https://developers.adobetarget.com/api/#create-xt-activity) | 新版 API 對 AB 和 XT 提供不同的建立方法 |
|  | 行銷活動更新 | [更新 AB 活動](https://developers.adobetarget.com/api/#update-ab-activity)<P>[更新 XT 活動](https://developers.adobetarget.com/api/#update-xt-activity) |  |
|  | 複製行銷活動 | 不適用 | 使用活動建立 API |
|  | 促銷活動清單 | [列出活動](https://developers.adobetarget.com/api/#list-activities) |  |
|  | 行銷活動狀態 | [更新活動狀態](https://developers.adobetarget.com/api/#update-activity-state) |  |
|  | 促銷活動檢視 | [依 ID 取得 AB 活動](https://developers.adobetarget.com/api/#get-ab-activity-by-id)<P>[依 ID 取得 XT 活動](https://developers.adobetarget.com/api/#get-xt-activity-by-id) |  |
|  | 第三方行銷活動 ID | 不適用 | 如果您使用 thirdpartyID，則可使用相關的活動方法 |
| 選件 | 選件建立 | [建立選件](https://developers.adobetarget.com/api/#create-offer) |  |
|  | 選件取得 | [依 ID 取得選件](https://developers.adobetarget.com/api/#get-offer-by-id) |  |
|  | 選件清單 | [列出選件](https://developers.adobetarget.com/api/#list-offers) |  |
|  | 資料夾清單 | 不適用 | [!DNL Target Standard/Premium]不支援資料夾 |
| 報表 | 行銷活動效能報表 | [取得 AB 效能報表](https://developers.adobetarget.com/api/#get-ab-performance-report)<P>[取得 XT 效能報表](https://developers.adobetarget.com/api/#get-xt-performance-report) |  |
|  | 稽核報表 | [取得稽核報告](https://developers.adobetarget.com/api/#get-audit-report) |  |
|  | 1-1 內容報表 | [取得 AP 效能報表](https://developers.adobetarget.com/api/#get-ap-activity-performance-report) |  |
| 帳戶設定 | 取得主機群組 | [列出環境](https://developers.adobetarget.com/api/#list-environments) |  |

## 例外

如果需要例外狀況，請聯絡您的「客戶成功經理」。

## 說明

如果您有任何問題，或需要協助從[!DNL Adobe Developer Console]上的Classic API轉換為[!DNL Target] API，請聯絡[!DNL Adobe Target Client Care] (tt-support@adobe.com)。
