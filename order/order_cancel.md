---
title: Cancel Order
keywords: Tiger Secuirties、Tiger API、 Open API
description: Tiger Open API help document
---

### Cancel Order

Request Parameter：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account   | string  |  Yes  |Trading account ID:DU575569
id  | long    |  Yes  |Order Number, ID number when placing an order

Response Result：

Name | Type | Description 
--- | --- | ---
id    | long | Unique ID , used to search order/change order/cancel order 


Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-09-10 10:00:00",
  "method":"cancel_order",
  "biz_content":"{
        \"id\":\"147070683398615040\",
        \"account\":\"DU575569\"
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.CANCEL_ORDER);

String bizContent = TradeParamBuilder.instance()
    .account("DU575569")
    .id(147070683398615040L)
    .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response Sample:

```json
{
    "code": 0,
    "message": null,
    "timestamp": 1525938835697,
    "data": {
        "id":147070683398615040
    }
}
```
