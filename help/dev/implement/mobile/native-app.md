---
keywords: 行動應用程式，aep sdk，原生應用程式，網頁檢視，原生；swift，adobe experience platform mobile sdk，行動sdk，原生程式碼
description: 瞭解如何實作 [!DNL Adobe Target] 使用 [!DNL AEP Mobile SDK] 在具有Web檢視的原生應用程式中。
title: 實作 [!DNL Adobe Target] 在搭配Web檢視使用原生程式碼的行動應用程式中
feature: Implement Mobile
role: Developer
source-git-commit: c0fda36cb5472d71438c47b8b484716003da4214
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# 實作 [!DNL Target] 使用 [!DNL AEP Mobile SDK] 在具有Web檢視的原生應用程式中

本文分享實施的最佳實務 [!DNL Adobe Target] 在透過網頁檢視使用原生程式碼的行動應用程式中，使用 [!DNL Adobe Experience Platform Mobile SDK].

本文使用範例iOS應用程式，使用 [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank} and a [!DNL Target] integration written in [Swift from the GitHub repository](https://github.com/adobe/aep-sdk-app/){target=_blank}.

在真實世界中，您的企業應用程式可能會使用行動應用程式中的網頁檢視。 網頁檢視是使用URL載入網頁的容器。 此容器類似於沒有控制項的瀏覽器視窗。 在iOS中，處理網頁時， Web檢視容器會當作Safari瀏覽器運作。

## 必要條件

若要開始使用 [!DNL Adobe Experience Platform Mobile SDK]，您必須執行一些必備作業。

如需詳細資訊，請參閱 [Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank} in the [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank} 檔案。

## 將原生程式碼與網頁檢視同步

實作時的挑戰 [!DNL Target] 在具有Web檢視的原生應用程式中， [!DNL Adobe Experience Platform Mobile SDK] 已產生所有需要的識別碼， [!DNL Adobe] 解決方案提供順暢運作。 不過，Web檢視尚未顯示識別碼，因為這些識別碼不在原生Platform環境中。 因此，您必須建立橋接器以將某些SDK識別碼傳遞至Web檢視，好讓訪客識別碼持續存在至Web環境中。 若未正確執行此操作，會導致重複造訪，進而影響您的報表。

幸運的是， [!DNL Adobe Experience Platform Mobile SDK] 提供便利的產生方法 [!DNL Adobe] Web檢視需要的引數，可供相同訪客使用和持續儲存，如下列範常式式碼所示：

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  print("appendedURL \(String(describing: appendedURL))")
  // load the url with ECID on the main thread
  DispatchQueue.main.async {
    let request = NSMutableURLRequest(url: appendedURL!)
    self.webView.load(request as URLRequest)
  }
});
```

如需關於的詳細資訊 `Identity.appendTo` 方法，若要檢視如何使用方法的範例，請參閱 [Swift >範例](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank} 在 *Mobile SDK檔案*.

使用 `Identity.appendTo`，此URL：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

轉換成：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

如您所見， `adobe_mc` 引數附加至URL。 此引數包含下列專案的編碼值：

* TS=1660667205：目前的時間戳記。 此時間戳記可確保網頁檢視不會收到過期的值。
* MCMID=69624092487065093697422606480535692677： [!UICONTROL EXPERIENCE CLOUDID] (ECID)。 也稱為MID或 [!UICONTROL MARKETING CLOUDID] 下列專案需要： [!DNL Adobe] 跨解決方案訪客身份識別。
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg： [!UICONTROL Adobe組織ID].

此 `Identity.getUrlVariables` 是替代方案 [!DNL Adobe Experience Platform Mobile SDK] 此方法會傳回適當格式的字串，該字串包含 [!DNL Experience Cloud Identity Service] URL變數。 如需詳細資訊，請參閱 [getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank} 在 *身分API參考*.

## 傳遞 [!DNL Target] 相同工作階段體驗的工作階段ID

還需要一個額外的步驟，以完成 [!DNL Target] 使用者歷程可在原生和Web檢視中順暢地運作。 此步驟包括擷取及傳遞 [!DNL Target] 來自的工作階段ID [!DNL Adobe Experience Platform Mobile SDK] 至行動應用程式的網頁檢視。

此 `Target.getSessionId` 擷取可以傳遞至網頁檢視URL的工作階段ID，做為 `mboxSession` 引數：

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## 在網頁檢視中測試

網頁預覽連結產生於 [!UICONTROL 活動詳細資料] 頁面，方法是按一下 [[!UICONTROL ADOBEQA] 連結](/help/dev/implement/mobile/target-mobile-preview.md) ，以顯示快顯視窗來複製每個體驗預覽連結，如下所示：

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

網頁預覽連結包含其他 `at_preview_index` 和 `at_preview_listed_activities_only` 引數。 複製這些引數，以使用網頁連結引數建構方便使用的行動裝置預覽連結。

例如：

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

在iOS Safari瀏覽器中開啟連結後，您的應用程式會擷取您在 `AppDelegate` 類別類似下列範例：

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  print("url= \(String(describing: url.absoluteString))")
  //...
```

現在您已擷取應用程式中的所有必要引數，您可以在必要時將其傳遞至網頁：

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  let urlWithWebPreviewLink = appendedURL + "&" + myPreviewLinkFromAppDelegate
```

Web檢視連結的最終輸出可能如下所示：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg&at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```
