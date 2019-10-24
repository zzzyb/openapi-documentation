---
title: Stock Kline Data
keywords: Tiger Securities、Tiger API、 Open API
description: Tiger Open API help document
---

### Request Kline Data

Request Parameter： 

 Parameter  | Type    | If required | Description                                                  
--- | --- | --- | ---
symbols|array|Yes|Stock symbol list, max limit：50
period|string|Yes|Kline type, range of values (day: day K, week: week K, month: month K, year: year K, 1 min: 1 minute, 5 min: 5 minutes, 15 min: 15 minutes, 30 min: 30 minutes, 60 min: 60 minute)
right|string|No|Re-right option, br: pre-recovery (default), nr: no re-right
begin_time|long|No|Start time, default: -1, unit: millisecond (ms), before closing and opening interval, that is, the query result will contain the start time data, such as querying the week/month/year K line, it will return the data containing the current period (such as : The starting time is Wednesday and will return from the K line on this Monday)
end_time|long|No|End time, default: -1, unit: millisecond (ms)
limit|integer|No|Quantity limit, default: 300
lang|string|No|Language supported: zh_CN,zh_TW,en_US, default: en_US


Response Result：

 Name   | Example | Description              
--- | --- | ---
symbol	|string|Stock symbol
period	|string|Kline period
items	|array| Ktime array, field detail instructions below 

Kline Data

Kline Field|Type|Description
---|---|---
close|double|Closing price
high|double|Highest price
low|double|Lowest price
open|double|Open maket price
time|long|Time
volume|long|Trade Volume

Sample Request:
```json    
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"kline",
  "biz_content":"{\"symbols\":[\"AAPL\"],\"period\":\"day\",\"begin_time\":1540785600000,\"limit\":100}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
QuoteKlineResponse response = client.execute(QuoteKlineRequest.newRequest(List.of("AAPL"), KType.day, "2018-10-01", "2018-12-25")
        .withLimit(1000)
        .withRight(RightOption.br));
if (response.isSuccess()) {
  System.out.println(Arrays.toString(response.getKlineItems().toArray()));
} else {
  System.out.println("response error:" + response.getMessage());
}
```

Response Sample:
```json    
{
  "code": 0,
  "message": "success",
  "timestamp": 1525938835697,
  "data": {
    "symbol": "AAPL",
    "period": "day",
    "items": [
      {
        "time": 1523851200000,
        "volume": 21578420,
        "open": 175.02999877929688,
        "close": 175.82000732421875,
        "high": 176.19000244140625,
        "low": 174.8300018310547
      },
      {
        "time": 1523937600000,
        "volume": 26597070,
        "open": 176.49000549316406,
        "close": 178.24000549316406,
        "high": 178.93600463867188,
        "low": 176.41000366210938
      }
    ]
  }
}
```

Kline Retention Time：

Market|Kline period|Retention time
--- | --- | ---
CN|1min\5min\15min\30min\60min\120min| One year 
CN|day\week\month\year| Unlimited 
US/HK|1min\5min|Recent 28 days
US/HK|15min\30min\60min\120min| One year 
US/HK|day\week\month\year|Unlimited
