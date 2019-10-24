### Request Transaction

Request Parameter：

 Parameter   | Type    | If required | Description                              
--- | --- | --- | ---
symbols|array|Yes|Stock symbol list
begin_index|integer|No| start index，default0 
end_index|integer|No|end index，default200
limit|integer|No|Quantity，default200
lang|string|No|Language supported: zh_CN,zh_TW,en_US, default: en_US


Response Result：： 

Name|Type|Description
--- | --- | ---
time	|long	|Transaction timestamp
price	|double	|Trade price
volume|	long	|Trade volume
type	|string	|Unchange means *, increase means +, decrease means -
beginIndex|	integer	|Return to beginning index data
endIndex	|integer	|Return to ending index data

Sample Request:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"trade_tick",
  "biz_content":"{\"symbols\":[\"AAPL\"],\"limit\":100}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
QuoteTradeTickResponse response = client.execute(QuoteTradeTickRequest.newRequest(List.of("AAPL")));
if (response.isSuccess()) {
  System.out.println(Arrays.toString(response.getTradeTickItems().toArray()));
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
    "items": [
      {
        "time": 1452827921850,
        "price": 4.1,
        "volume": 300,
        "type": "*"
      },
      {
        "time": 1452827922850,
        "price": 4.12,
        "volume": 1200,
        "type": "+"
      },
      {
        "time": 1452827923850,
        "price": 4.08,
        "volume": 2000,
        "type": "-"
      }
    ],
    "beginIndex": 0,
    "endIndex": 3
  }
}
```
