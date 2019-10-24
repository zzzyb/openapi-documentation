### Request Stock Symbol List and Name

Request Parameter：

 Parameter | Type   | If required | Description                              
---|---|---|---
market|string|Yes|US stocks, HK stocks, CN A stocks, ALL
lang|string|No|Language supported: zh_CN,zh_TW,en_US, default: en_US

Response Result：

 Name   | Type   | Description  
---|---|---
name|string|Stock Name
symbol|string|Stock Symbol

Sample Request:
```json 
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"all_symbol_names",
  "biz_content":"{\"market\": \"US\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
QuoteSymbolNameResponse response = client.execute(QuoteSymbolNameRequest.newRequest(Market.US));
if (response.isSuccess()) {
  System.out.println(Arrays.toString(response.getSymbolNameItems().toArray()));
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
  "data": [{"name":"CKH Holdings","symbol":"00001"},{"name":"CLP","symbol":"00002"}]
}
```
