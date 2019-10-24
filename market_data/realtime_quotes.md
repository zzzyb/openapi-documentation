### Request real-time Market Data

Request Parameter：

 Parameter | Type   | If required | Description                              
--- | --- | --- | ---
symbols|array|Yes|Stock symbol list, max limit：50
lang|string|No|Language supported: zh_CN,zh_TW,en_US, default: en_US

Return Result：

Field | Type | Description 
--- | :-: | ---
symbol | string| Stock symbol 
open | double| Open price 
high | double| Highest price 
low | double| Lowest price 
close | double | Clossing price 
preClose| double| Yesterday closing price 
latestPrice | double | Latest price 
latestTime | long | Latest trade time 
latestSize | integer | Lastest trade volume 
askPrice| double | Ask price 
askSize| long | Ask volume 
bidPrice| double | Bidding price 
bidSize| long | Bidding size 
volume | long | Trade volume 
status | short | Market status 

Sample Request:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"quote_real_time",
  "biz_content":"{\"symbols\":[\"AAPL\"]}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:

```java
QuoteRealTimeQuoteResponse response = client.execute(QuoteRealTimeQuoteRequest.newRequest(List.of("AAPL")));
if (response.isSuccess()) {
  System.out.println(Arrays.toString(response.getRealTimeQuoteItems().toArray()));
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
      "latestPrice": 174.72,
      "askPrice": 173.79,
      "bidSize": 2,
      "bidPrice": 173.5,
      "volume": 43098410,
      "high": 174.78,
      "preClose": 176.69,
      "low": 170.42,
      "latestTime": 1544130000000,
      "close": 174.72,
      "askSize": 1,
      "open": 171.76,
      "status": 0.0
    }
  ],
  "timestamp": 1544178615507,
  "message": "success"
}
```
