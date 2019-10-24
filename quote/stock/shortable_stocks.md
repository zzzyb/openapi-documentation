---
title: Shortable stock Data
keywords: Tiger Securities、Tiger API、 Open API
description: Tiger Open API help document
---

### Request Shortable Stock Data

Request Parameter：

 Parameter | Type   | If required | Description                              
--- | --- | --- | ---
symbols |array|Yes|Stock symbol list, max limit: 100
lang|string|No|Language supported: zh_CN,zh_TW,en_US, default: en_US


Response Result：

 Name           | Type   | Description              
--- | --- | ---
symbol|	string| Stock symbol 
daysToCover|	Double| Days to cover 
avgDailyVolume| Long  | Daily trade volume 
shortInterest|	Long	|Open short interest
settlementDate|	String	|Date, format：yyyy-MM-dd
percentOfFloat|	Double	|Percentage of float


Sample Request:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"quote_shortable_stocks",
  "biz_content":"{\"symbols\":[\"AAPL\",\"GOOG\"]}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:
```java
QuoteShortableStockResponse response = client.execute(QuoteShortableStockRequest.newRequest(List.of("AAPL")));
if (response.isSuccess()) {
  System.out.println(Arrays.toString(response.getShortableStockItems().toArray()));
} else {
  System.out.println("response error:" + response.getMessage());
}
```

Response Sample:
```json
{
  "code": 0,
  "data": [
    {
      "symbol": "AAPL",
      "items": [
        {
          "daysToCover": 2.0,
          "percentOfFloat": 1.0,
          "shortInterest": 47549672,
          "settlementDate": "2018-08-31",
          "avgDailyVolume": 28293025
        },
        {
          "daysToCover": 3.0,
          "percentOfFloat": 0.9,
          "shortInterest": 42845978,
          "settlementDate": "2018-07-13",
          "avgDailyVolume": 16727009
        },
        {
          "daysToCover": 2.0,
          "percentOfFloat": 0.8,
          "shortInterest": 38080068,
          "settlementDate": "2018-06-29",
          "avgDailyVolume": 24684978
        }
      ]
    }
  ],
  "timestamp": 1544411829927,
  "message": "success"
}
```
