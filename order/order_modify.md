### Modify Order

Request Parameter:

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading account ID:DU575569
id            |long    |  Yes  |Order id，returned when order placed
total_quantity|int     |  No  |Order Quantity (there is a minimum quantity limit for Hong Kong stocks, shanghai-hong kong stock connect, warrant and bull/bear certificate)
order_type	  |string  |  No  |Order Type. MKT (market order）, LMT (limit order）, STP (stop order), STP_LMT (stop limit order), TRAIL (trailing stop order)
limit_price	  |double  |  No  |Limit price, required when order_type is LMT,STP,STP_LMT
aux_price	  |double  |  No  |Stop loss price of stock. This parameter is required when order_type is STP,STP_LMT
time_in_force |string  |  No  |Order expiration date，only DAY（valid during day）aGTC（Valid before cancellation），default DAY


Response Result:

Name | Type | Description 
--- | --- | ---
id    | long | Unique order number, can be used to query orders/modify orders/cancel orders 

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-09-10 10:00:00",
  "method":"modify_order",
  "biz_content":"{
      \"id\":\"147070683398615040\",
      \"account\":\"DU575569\",
      \"order_type\":\"LMT\",
      \"limit_price\": 9.0,
      \"total_quantity\": 10,
      \"time_in_force\": \"DAY\"
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.MODIFY_ORDER);

String bizContent = TradeParamBuilder.instance()
    .account("DU575569")
    .id(147070683398615040L)
    .symbol("AAPL")
    .totalQuantity(200)
    .limitPrice(60.0)
    .orderType(OrderType.LMT)
    .action(ActionType.BUY)
    .secType(SecType.STK)
    .currency(Currency.USD)
    .outsideRth(false)
    .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response Request:

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
