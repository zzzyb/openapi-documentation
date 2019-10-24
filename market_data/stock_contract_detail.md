---
title: Stock Trade Information
keywords: Tiger Securities、Tiger API、 Open API
description: Tiger Open API help document
---

### Request Stock Trade Information

Request Parameter：

 Parameter | Type  | If required | Description               
--- | --- | --- | ---
symbols |array|Yes|Stock symbol list, max limit：100


Response Result：

 Name        | Type    | Description       
--- | --- | ---
symbol|	string| Stock symbol 
lotSize|	Integer| Board lot size 
spreadScale| Integer  | Quote spread 
minTick|	Double	|Mininum tick size


Sample Request:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"quote_stock_trade",
  "biz_content":"{\"symbols\":[\"AAPL\",\"GOOG\"]}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:
```java
List<String> symbols = new ArrayList<>();
symbols.add("00700");
symbols.add("00810");
QuoteStockTradeResponse response = client.execute(QuoteStockTradeRequest.newRequest(symbols));
if (response.isSuccess()) {
  System.out.println(Arrays.toString(response.getStockTradeItems().toArray()));
} else {
  System.out.println("response error:" + response.getMessage());
}
```

Response Sample:
```json
{
	"code": 0,
	"data": [{
		"lotSize": 100,
		"minTick": 0.2,
		"spreadScale": 0,
		"symbol": "00700"
	}, {
		"lotSize": 6000,
		"minTick": 0.001,
		"spreadScale": 0,
		"symbol": "00810"
	}],
	"message": "success",
	"timestamp": 1546853907390
}
```
