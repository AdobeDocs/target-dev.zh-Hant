---
keywords: 行動應用程式，aep sdk，原生應用程式，網頁檢視，原生；swift，adobe experience platform mobile sdk，行動sdk，原生程式碼
description: 瞭解如何在具有Web檢視的原生應用程式中使用 [!DNL AEP Mobile SDK] 實作 [!DNL Adobe Target] 。
title: 在使用原生程式碼搭配Web檢視的行動應用程式中實作 [!DNL Adobe Target]
feature: Implement Mobile
role: Developer
exl-id: 3dd2e1d7-c744-4ba8-aaa4-6c2fe64d01fa
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# 透過Web檢視在原生應用程式中使用[!DNL AEP Mobile SDK]實作[!DNL Target]

本文分享在行動應用程式中實作[!DNL Adobe Target]的最佳作法，該應用程式使用原生程式碼，並使用[!DNL Adobe Experience Platform Mobile SDK]搭配Web檢視。

本文使用範例iOS應用程式，使用GitHub存放庫](https://github.com/adobe/aep-sdk-app/){target=_blank}的[[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank}以及以[Swift撰寫的[!DNL Target]整合。

在真實世界中，您的企業應用程式可能會使用行動應用程式中的網頁檢視。 網頁檢視是使用URL載入網頁的容器。 此容器類似於沒有控制項的瀏覽器視窗。 在iOS中，處理網頁時， Web檢視容器會當作Safari瀏覽器運作。

## 必備條件

若要開始使用[!DNL Adobe Experience Platform Mobile SDK]，您必須執行一些先決條件工作。

如需詳細資訊，請參閱[[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank}檔案中的[Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank}。

## 將原生程式碼與網頁檢視同步

在具有Web檢視的原生應用程式中實作[!DNL Target]時，難題是[!DNL Adobe Experience Platform Mobile SDK]已產生[!DNL Adobe]解決方案無縫運作所需的所有必要識別碼。 不過，Web檢視尚未顯示識別碼，因為這些識別碼不在原生Platform環境中。 因此，您必須建立橋接器以將某些SDK識別碼傳遞至Web檢視，好讓訪客識別碼持續存在至Web環境中。 若未正確執行此操作，會導致重複造訪，進而影響您的報表。

幸運的是，[!DNL Adobe Experience Platform Mobile SDK]提供了一種便利的方法，可產生Web檢視所需的[!DNL Adobe]引數，以供相同訪客使用及持續儲存，如下列範常式式碼所示：

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

如需`Identity.appendTo`方法的詳細資訊，以及方法使用範例，請參閱&#x200B;*Mobile SDK檔案*&#x200B;中的[Swift >範例](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank}。

使用`Identity.appendTo`，此URL：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

轉換成：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

如您所見，URL後面已附加`adobe_mc`引數。 此引數包含下列專案的編碼值：

* TS=1660667205：目前的時間戳記。 此時間戳記可確保網頁檢視不會收到過期的值。
* MCMID=69624092487065093697422606480535692677： [!UICONTROL Experience Cloud ID] (ECID)。 也稱為[!DNL Adobe]跨解決方案訪客身分識別所需的MID或[!UICONTROL Marketing Cloud ID]。
* mcorgid=EB9CAE8B56E003697F000101@AdobeOrg： [!UICONTROL Adobe Organization ID]。

`Identity.getUrlVariables`是替代[!DNL Adobe Experience Platform Mobile SDK]方法，可傳回包含[!DNL Experience Cloud Identity Service] URL變數的適當格式字串。 如需詳細資訊，請參閱&#x200B;*身分API參考*&#x200B;中的[getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank}。

## 傳遞相同工作階段體驗的[!DNL Target]工作階段識別碼

需要額外一個步驟，才能讓[!DNL Target]使用者歷程在原生和Web檢視之間順暢地運作。 此步驟包括從[!DNL Adobe Experience Platform Mobile SDK]擷取[!DNL Target]工作階段ID並將其傳遞至行動應用程式的網頁檢視。

`Target.getSessionId`會擷取工作階段ID，其可作為`mboxSession`引數傳遞至Web檢視URL：

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## 在網頁檢視中測試

在[!UICONTROL Activity detail]頁面上按一下[[!UICONTROL Adobe QA]連結](/help/dev/implement/mobile/target-mobile-preview.md)以顯示快顯視窗來複製每個體驗預覽連結，會產生網頁預覽連結，如下所示：

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Web預覽連結包含其他`at_preview_index`和`at_preview_listed_activities_only`引數。 複製這些引數，以使用網頁連結引數建構方便使用的行動裝置預覽連結。

例如：

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

在iOS Safari瀏覽器中開啟連結後，您的應用程式會擷取您`AppDelegate`類別中的URL，類似下列範例：

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
