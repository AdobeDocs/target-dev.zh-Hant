---
keywords: api， adobe i/o， classic， adobe developer console
description: 瞭解如何從轉換 [!DNL Adobe Target Classic] 的API [!DNL Target] 上的API [!DNL Adobe Developer Console].
title: 如何轉換自 [!DNL Target Classic] API至 [!DNL Target] 上的API [!DNL Adobe Developer Console]？
feature: APIs/SDKs
exl-id: b84e3767-89ad-4e2d-9bb4-7e31bffbc285
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 34%

---

# 轉換自 [!DNL Target Classic] API至 [!DNL Target] 上的API [!DNL Adobe Developer Console]

可協助您轉換的資訊，從 [!DNL Target Classic] 的API [!DNL Target] 上的API [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

隨著停用 [!DNL Adobe Target Classic]，此類API已連線至您的 [!DNL Target Classic] 帳戶也因此無法使用。 本文會協助您轉換至 [!DNL Target Classic] API型整合，與 [!DNL Target] 由提供的API [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

有關詳細資訊 [!DNL Target] API，請參閱 [[!DNL Target] API](/help/dev/before-administer/target-api-overview.md). 有關詳細資訊 [!DNL Target] SDK，請參閱 [[!DNL Target] 伺服器端實作](/help/dev/implement/server-side/server-side-overview.md)

## 術語

| 術語 | 說明 |
|--- |--- |
| 傳統API | 連結至您的 [!DNL Target Classic] 帳戶。 這些 API 呼叫是根據使用者名稱和密碼式驗證，並使用主機名稱 `testandtarget.omniture.com`。如果您的API呼叫在請求URL中包含使用者名稱和密碼，您必須轉換至 [!DNL Adobe Developer Console] API。 |
| [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) | 此 [!DNL Adobe Developer Console] 是的閘道 [!DNL Target] API。 這些API已連線至您的 [!DNL Target Standard/Premium] 帳戶。 此 [!DNL Target] 上的API [!DNL Adobe Developer Console] 使用 [JWT型驗證](../../before-administer/configure-authentication.md)，此為安全企業API的業界標準。 |

## 時間表

下列API在下列情況下已終止服務： [!DNL Target Classic] 已終止服務：

| 日期 | 詳細資料 |
|--- |--- |
| 2017 年 10 月 17 日 | 已停止支援執行寫入作業的所有 API 方法 (`saveCampaign`、`copyCampaign`、`saveHTMLOfferContent` 以及 `setCampaignState`)。<P>這是全部發生的相同日期 [!DNL Target Classic] 使用者帳戶已設定為唯讀狀態。 |
| 2017 年 11 月 14 日 | 其餘 API 已停用。這是日期，當 [!DNL Target Classic] 使用者介面已解除委任 |

[!DNL Recommendations Classic] API不受此時間表影響。

## 對等方法

下表列出等同專案 [!DNL Adobe Developer Console] 傳統API方法的API方法。 此 [!DNL Adobe Developer Console] API會傳回JSON，而Classic API會傳回XML。

此 [!DNL Adobe Developer Console] API方法會連結至API檔案網站中的對應區段。 每一個 API 方法都有範例。您也可以使用 [!DNL Target] 管理API Postman集合，其中包含上所有AdobeAPI方法的範例API呼叫 [!DNL Adobe Developer Console].

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
|  | 資料夾清單 | 不適用 | 不支援的資料夾 [!DNL Target Standard/Premium] |
| 報表 | 行銷活動效能報表 | [取得 AB 效能報表](https://developers.adobetarget.com/api/#get-ab-performance-report)<P>[取得 XT 效能報表](https://developers.adobetarget.com/api/#get-xt-performance-report) |  |
|  | 稽核報表 | [取得稽核報表](https://developers.adobetarget.com/api/#get-audit-report) |  |
|  | 1-1 內容報表 | [取得 AP 效能報表](https://developers.adobetarget.com/api/#get-ap-activity-performance-report) |  |
| 帳戶設定 | 取得主機群組 | [列出環境](https://developers.adobetarget.com/api/#list-environments) |  |

## 例外

如果需要例外狀況，請聯絡您的「客戶成功經理」。

## 說明

請聯絡 [!DNL Adobe Target Client Care] (tt-support@adobe.com)如果您有任何問題，或需要協助從Classic API移轉至 [!DNL Target] 上的API [!DNL Adobe Developer Console].
