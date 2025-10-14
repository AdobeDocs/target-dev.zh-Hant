---
keywords: 行動應用程式, 行動應用程式位置, target mobile 應用程式, mobile target 位置, 行動應用程式成功量度
description: 檢視程式碼範例，協助您瞭解如何在iOS應用程式中建立位置和成功量度，以便使用 [!DNL Adobe Target] 個人化和最佳化您的應用程式。
title: 如何在iOS應用程式中建立 [!DNL Target] 位置和成功量度？
feature: Implement Mobile
exl-id: 755c8b26-5c60-48fc-9e7e-5e97a25edb78
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 66%

---

# iOS — 建立[!DNL Target]位置和成功量度

若要在您的行動應用程式中使用[!DNL Target]，請建立位置和成功量度。

>[!IMPORTANT]
>
>支援[!DNL Adobe Mobile]版本4。*x* SDK已於2021年8月31日結束，不建議再供[!DNL Adobe Target]個行動使用者使用。
>
>[適用於行動應用程式的Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank}是支援行動應用程式中[!DNL Adobe Experience Cloud]解決方案和服務的建議解決方案。

此區段包括可用作您的應用程式範本的樣本代碼。此區段中的樣本包含 iOS 的代碼。Android 適用相同的模式。您可以在[適用於Experience Cloud解決方案的Android SDK 4.x &#x200B;](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/target-main.html?lang=zh-Hant)指南中找到Android的特定語法。

>[!NOTE]
>
>請參閱[行動檔案](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-target-methods.html?lang=zh-Hant)，以取得所有可用[!DNL Target]方法的清單。

若要在您的應用程式中建立[!DNL Target]位置並進行要求，有兩個主要方法：

* `targetCreateRequestWithName`
* `targetLoadRequest`

1. 建立[!DNL Target]位置。

   這是用來建立要求的樣本呼叫:

   ```
   // make your request 
   ADBTargetLocationRequest *myRequest = [ADBMobile targetCreateRequestWithName:@"heroBanner" 
                                                    defaultContent:@"default.png" 
                                                    parameters:nil];
   ```

   | 參數 | 說明 |
   |---|---|
   | `ADBTargetLocationRequest *myRequest` | 將 `myRequest` 以應用程式中您的 `targetLocation` 名稱取代。 |
   | `targetCreateRequestWithName:@"heroBanner"` | 將 `heroBanner` 以 Target 中您的 `targetLocation` 名稱取代。這與 mbox 名稱相同。此英雄橫幅會出現在 Target 介面中。 |
   | `defaultContent:@"default.png"` | 以 Target 未回應時應用程式會使用的值取代 `default.png`。 |
   | `parameters:nil` | 指定設定檔或 mbox 參數。如需詳細資訊，請參閱「傳遞自訂資料」小節。 |

   這是用來載入要求的樣本呼叫:

   ```
   // load your request 
   [ADBMobile targetLoadRequest:myRequest callback:^(NSString *content) { 
                                        // do something with content 
                                        heroImage.image = [UIImage imageNamed:content]; 
   }];
   ```

   | 參數 | 說明 |
   |---|---|
   | `targetLoadRequest:myRequest` | 將 `myRequest` 以應用程式中您的 `targetLocation` 名稱取代。 |
   | `NSString *content` | 使用從 Adobe 傳回的實際內容取代內容。字串可以是 XML、JSON 或純字串。使用代碼的此區段來定義變數、設定影像路徑、檢視控制器流程、交易點，或您要執行的任何動作。Target 將以完全相同的格式傳回在 UI 中輸入的內容。 |
   | `heroImage.image = [UIImage imageNamed:content];` | 例如: 取得內容並設定英雄影像的路徑。 |

1. 建立成功量度。

   方法 `targetCreateOrderConfirmRequestWithName` 可用來在您的應用程式中追蹤轉換/成功量度。

   ```
   ADBTargetLocationRequest *req = [ADBMobile targetCreateOrderConfirmRequestWithName: "orderConfirm" 
                                              orderId: orderId 
                                              orderTotal: @"39.95" 
                                              productPurchasedId: _galleryItem.title 
                                              parameters: nil]; 
   [ADBMobile targetLoadRequest: req callback: nil];
   ```

   | 參數 | 說明 |
   |---|---|
   | `orderId` | 以代表不重複訂購 ID 的動態變數取代。 |
   | `@"39.95"` | 以代表不重複訂購總計的動態變數取代。 |
   | `_galleryItem.title` | 以代表所購買產品逗號分隔清單的動態變數取代。 |
   | `parameters: nil` | 其他參數的可選字典。 |

1. 建置應用程式。

   步驟結果: 成功建立目標位置並標記成功量度之後，請建立 A/B 測試。您可以使用表單型體驗撰寫器來建立活動。
