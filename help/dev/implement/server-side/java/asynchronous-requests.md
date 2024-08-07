---
title: 如何在 [!DNL Adobe Target] Java SDK中使用非同步要求
description: 瞭解 [!DNL Target] Java SDK如何支援非同步要求，將有效目標時間減少為零。
feature: APIs/SDKs
exl-id: e11f8d16-76f6-4d39-822a-34a1cf7f623f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 3%

---

# 非同步請求(Java)

## 說明

伺服器端整合的好處之一，就是您可以使用平行處理功能，在伺服器端利用龐大的頻寬和運算資源。 [!DNL Target] Java SDK支援非同步要求，因此可將有效目標時間減少為零。

## 支援的方法

### 方法

```javascript {line-numbers="true"}
CompletableFuture<TargetDeliveryResponse> getOffersAsync(TargetDeliveryRequest request);
CompletableFuture<ResponseStatus> sendNotificationsAsync(TargetDeliveryRequest request);
CompletableFuture<Attributes> getAttributesAsync(TargetDeliveryRequest targetRequest, String ...mboxes);
```

## 範例

範例`Spring`應用程式控制器可能如下所示：

### 範例控制器

```javascript {line-numbers="true"}
@RestController
public class TargetRestController {

    @Autowired
    private TargetClient targetJavaClient;

    @GetMapping("/mboxTargetOnlyAsync")
        public TargetDeliveryResponse mboxTargetOnlyAsync(
                @RequestParam(name = "mbox", defaultValue = "server-side-mbox") String mbox,
                HttpServletRequest request, HttpServletResponse response) {
            ExecuteRequest executeRequest = new ExecuteRequest()
                    .mboxes(getMboxRequests(mbox));

            TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                    .context(getContext(request))
                    .execute(executeRequest)
                    .cookies(getTargetCookies(request.getCookies()))
                    .build();
            CompletableFuture<TargetDeliveryResponse> targetResponseAsync =
                    targetJavaClient.getOffersAsync(targetDeliveryRequest);
            targetResponseAsync.thenAccept(tr -> setCookies(tr.getCookies(), response));
            simulateIO();
            TargetDeliveryResponse targetResponse = targetResponseAsync.join();
            return targetResponse;
        }

    /**
     * Function for simulating network calls like other microservices and database calls
     */
    private void simulateIO() {
        try {
            Thread.sleep(200L);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}
```

此範例假設您已[將SDK](initialize-sdk.md)初始化為spring bean，且您有[可用的公用程式方法](utility-methods.md)。

[!DNL Target]要求是在`simulateIO`之前觸發，且在執行時，目標結果也應準備就緒。 即使不是如此，在大多數情況下您還是可以節省大量成本。
