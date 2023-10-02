---
keywords: qa，預覽，預覽連結，行動裝置，行動裝置預覽
description: 使用行動裝置預覽連結來為行動應用程式活動執行端對端品質保證。
title: 如何在中使用行動裝置預覽連結 [!DNL Adobe Target] 行動裝置？
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
source-git-commit: 0bcfa16cb79644e7ce10e33daf6c8385104c197f
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 27%

---

# [!DNL Target] 行動裝置預覽

使用行動裝置預覽連結可針對行動應用程式活動執行簡單的端對端品質保證，並且無需任何特殊測試裝置，即可在使用裝置的情況下註冊不同的體驗。

行動裝置預覽功能可讓您在啟動行動應用程式活動之前，完整測試這些活動。

## 必要條件

1. **使用支援的SDK版本：** 行動裝置預覽功能需要您下載並安裝適當版本的 [!DNL Adobe Mobile SDK] （在您對應的應用程式中）。

   如需下載適當SDK的說明，請參閱 [目前SDK版本](https://developer.adobe.com/client-sdks/documentation/current-sdk-versions/){target=_blank} 在 *[!DNL Adobe Experience Platform Mobile SDK]* 檔案。

1. **設定 URL 配置:** 預覽連結使用 URL 配置來開啟您的應用程式。指定預覽的唯一URL配置。

   如需詳細資訊，請參閱 [視覺預覽](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} 在 *在資料連線UI中設定Target擴充功能* 在 *[!DNL Mobile SDK]* 檔案。

   下列連結包含更多資訊：

   * **iOs**：如需為iOS設定URL配置的詳細資訊，請參閱 [為您的應用程式定義自訂URL配置](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app){target=_blank} 於 *Apple開發人員* 網站。
   * **Android**：如需為Android設定URL配置的詳細資訊，請參閱 [建立應用程式內容的深層連結](https://developer.android.com/training/app-links/deep-linking){target=_blank} 於 *Android開發人員* 網站。

1. **設定 `collectLaunchInfo` API （僅限i0S）**

   如需詳細資訊，請參閱 [視覺預覽](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} 在 *在資料連線UI中設定Target擴充功能* 在 *[!DNL Mobile SDK]* 檔案。

## 產生預覽連結

1. 在 [!DNL Target] UI，按一下 **[!UICONTROL 更多選項]** 圖示（垂直省略符號），然後選取「 」 **[!UICONTROL 建立行動裝置預覽連結]**.

   ![替代影像](assets/mobile-preview-create.png)

1. 選取您要預覽的活動，然後按一下 **[!UICONTROL 產生行動裝置預覽連結]**.

   >[!NOTE]
   >
   >您只能選取表單式 [!UICONTROL A/B測試] 和 [!UICONTROL 體驗鎖定] (XT)活動。

   ![替代影像](assets/mobile-preview-select-activities.png)

1. 指定您應用程式的 URL 配置。

   此URL配置必須與iOS或Android應用程式中存在的URL配置相同。 如有必要，請分別對iOS和Android重複此程式。

   ![替代影像](assets/mobile-preview-enter-url-scheme.png)

1. 按一下&#x200B;**[!UICONTROL 「產生行動裝置預覽連結」]**，然後複製連結。

   ![替代影像](assets/mobile-preview-generate-and-copy.png)

## 在您的裝置上預覽

在您安裝應用程式所在裝置上的行動瀏覽器中開啟連結。此應用程式可能是您從下載的生產應用程式 [!DNL Apple App Store] 或 [!DNL Google Play Store]. 應用程式不需要是特殊的組建。 如果您有作用中的預覽連結，則可以在裝置上檢視體驗。

1. 在您的行動瀏覽器中開啟連結。

   分享您在上一節中複製的連結，來自 [!DNL Target] 以方便的方式將UI連線至行動裝置，例如使用文字、電子郵件或 [!DNL Slack].

   |![預覽深層連結 1](assets/mobile-preview-open-deeplink.png)|![預覽深層連結 2](assets/mobile-preview-open-app.png)|

   您的應用程式隨即開啟並啟動 [!DNL Target] [!UICONTROL 行動裝置預覽模式].

1. 選取您要查看的體驗組合，然後按一下&#x200B;**[!UICONTROL 「啟動體驗」]**。

   |![行動裝置預覽 1](assets/mobile-preview-experience-selection-1.png)|![行動裝置預覽 2](assets/mobile-preview-experience-result-1-france.png)|![行動裝置預覽 3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![行動裝置預覽 4](assets/mobile-preview-experience-selection-2.png)|![行動裝置預覽 5](assets/mobile-preview-experience-result-2-aus.png)|![行動裝置預覽 6](assets/mobile-preview-experience-result-2-10off.png)|

## 限制

* 按一下&#x200B;**[!UICONTROL 「啟動體驗」]**&#x200B;按鈕之後，必須重新載入檢視新內容才會顯示。最容易的方式是切換至不同畫面，然後回到您預期會發生變更的畫面。
* Android 早於 API-19 (KitKat) 的版本不支援行動裝置預覽。
