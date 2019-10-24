### Request Timeline Data

Request Parameter： 

 Parameter            | Type    | If required | Description                              
--- | --- | --- | ---
symbols|array|Yes|Stock symbol list
period|string|Yes|Timeline period，include：day and day5
include_hour_trading|boolean|No|Whether to include pre-market and after-market data，default：false
begin_time|long|No|Start time (millisecond timestamp), which returns the day's data by default
lang|string|No|Language supported: zh_CN,zh_TW,en_US, default: en_US

Response Result：

Name|Type|Description
--- | --- | ---
symbol|string|Stock symbol
period|string|Period day or 5day
preClose|double|Yesterday closing price
intraday| object | Intraday time-sharing array, field reference below 
preMarket|object|(US stock only) Pre-market time-sharing array and start-end time, field reference below
afterHours|	object| (Us stock only)   After-market time-sharing array and start-end time, field reference below 

Timeline Data

Timeline Field|Description
--- | -- 
volume| Trade volume 
avgPrice| Average trade price 
price| Latest price 
time| current time-sharing time 

Sample Request:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"timeline",
  "biz_content":"{\"symbols\":[\"AAPL\"],\"period\":\"day\",\"begin_time\":1544129760000}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
QuoteTimelineResponse response = client.execute(QuoteTimelineRequest.newRequest(List.of("AAPL"), 1544129760000L));
if (response.isSuccess()) {
  System.out.println(Arrays.toString(response.getTimelineItems().toArray()));
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
      "preMarket": {
        "endTime": 1544106600000,
        "beginTime": 1544086800000,
        "items": [
           
        ]
      },
      "period": "day",
      "preClose": 176.69000244140625,
      "afterHours": {
        "endTime": 1544144400000,
        "beginTime": 1544130000000,
        "items": [
          {
            "volume": 872772,
            "avgPrice": 174.71916,
            "price": 174.69,
            "time": 1544130000000
          },
          {
            "volume": 3792,
            "avgPrice": 174.71893,
            "price": 174.66,
            "time": 1544130060000
          }
        ]
      },
      "intraday": [
        {
          "items": [
            {
              "volume": 201594,
              "avgPrice": 172.0327,
              "price": 174.34,
              "time": 1544129760000
            },
            {
              "volume": 139040,
              "avgPrice": 172.03645,
              "price": 174.4156,
              "time": 1544129820000
            },
            {
              "volume": 178427,
              "avgPrice": 172.0413,
              "price": 174.44,
              "time": 1544129880000
            },
            {
              "volume": 2969567,
              "avgPrice": 172.21619,
              "price": 174.72,
              "time": 1544129940000
            }
          ]
        }
      ]
    }
  ],
  "timestamp": 1544185099595,
  "message": "success"
}
```

