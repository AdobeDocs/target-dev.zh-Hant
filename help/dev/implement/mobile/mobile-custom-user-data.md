---
keywords: 行動應用程式, 行動應用程式傳送資料, target 行動應用程式, 行動自訂使用者資料, 行動應用程式自訂資料
description: 瞭解如何以名稱 — 值組的形式將關於位置或使用者的其他資訊傳送至 [!DNL Adobe Target] ，以協助您建立自訂對象。
title: 如何在iOS應用程式中傳送自訂使用者資料？
feature: Implement Mobile
exl-id: 9cf8e8fd-1898-43b1-b339-d7a21cb35d57
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 55%

---

# iOS - 傳送自訂使用者資料

您可以以名稱 — 值組的方式將關於位置或使用者的其他資訊傳送到[!DNL Target]。

>[!IMPORTANT]
>
>支援[!DNL Adobe Mobile]版本4。*x* SDK已於2021年8月31日結束，不建議再供[!DNL Adobe Target]個行動使用者使用。
>
>[適用於行動應用程式的Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank}是支援行動應用程式中[!DNL Adobe Experience Cloud]解決方案和服務的建議解決方案。

此資訊可用來建立自訂對象 (例如，大於 25000 英里的使用者) 以及在報表中使用。

您可以透過[!DNL Target]呼叫傳送兩種型別的引數：

* **mbox引數**： mbox引數不是跨工作階段持續儲存的。
* **設定檔引數**：設定檔引數儲存在訪客設定檔存放區中，且跨工作階段持續儲存。 mbox引數不會持續存在。 雖然會保留一些金鑰，設定檔和 mbox 參數均可以是自訂機碼值組。

儘管有一些保留的金鑰，設定檔和 mbox 參數均可以包含自訂機碼值組。

1. 建立字典。

   首先，使用您傳送以傳遞至[!DNL Target]的值建立字典。 為了方便，請在 `welcomeMessageCampaign` 方法內新增此項目，因此您不需擔心範圍。

   下列是樣本字典。您可以在 `(void)welcomeMessageCampaign` 內複製貼上這個。在此範例中，`userLevel` 和 `userMiles` 之類金鑰的值會加上硬式編碼。一般來說，您會傳入對應的變數。

   ```
   NSDictionary *targetParams = [[NSDictionary alloc] initWithObjectsAndKeys: 
                                 @"platinum",@"userLevel", 
                                 @26500,@"userMiles", 
                                 @"1067007",@"entity.id", 
                                 @"dealsapp.qa", @"host", 
                                 @"fashion",@"entity.categoryId", 
                                 @"millenial", @"profile.persona", 
                                 @"cohort_5", @"profile.cohort", 
                                 nil];
   ```

   * 具有 profile 字首的金鑰 (例如，`profile.persona`) 會儲存在使用者的設定檔上。

     您可以在不同活動和通道間使用這些設定檔屬性。

   * 沒有任何字首的金鑰 (例如，`userMiles`) 為 mbox 參數。

     這些參數只能在工作階段期間使用。

   * 具有字首 entity 的金鑰 (例如，`entity.category.id`) 則用於產品建議。

1. 驗證資料。
   1. 在應用程式 `didFinishLaunchingWithOptions` 中，取消註解或新增 `[ADBMobile setDebugLogging:YES];`。

      這會列印詳細的偵錯記錄。
   1. 建置應用程式。
   1. 驗證參數已傳入目標呼叫中。

      在您的偵錯主控台搜尋您的目標位置名稱。您將看到對 `YOUR-CLIENT-CODE.tt.omtrdc.net` 的呼叫包含剛才傳入的所有參數。

      （按一下影像可展開至完整寬度。）

      偵錯主控台中的![目標位置](/help/dev/implement/mobile/assets/mobile-debug.png "偵錯主控台中的Target位置"){zoomable="yes"}

   您可以在[!DNL Target]中使用這些引數來建立對象，以及限制或鎖定內容的顯示。
