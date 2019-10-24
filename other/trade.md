### API Instruction
Trading API mainly provides interfaces for placing orders, canceling orders, and modifying orders. 

!!! note

* You need to get the order number before you place an order and pass it along when you place the order
* The duplicate order number will lead to the failure of placing orders. The purpose of this API is to prevent users from placing duplicate orders and causing unnecessary losses to users due to the network and other reasons

### Request Order Number

Request Parameter：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading account ID:DU575569

Response Result:

Name | Type | Decription 
--- | --- | ---
orderId  | int  | Order ID, in order to prevent duplicate orders, you need to pass this parameter when placing an order. 

Request Sample:

```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-09-10 10:00:00",
  "method":"order_no",
  "biz_content":"{\"account\":\"DU575569\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

 SDK Request Sample:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey, publicKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.ORDER_NO);

String bizContent = TradeParamBuilder.instance()
    .account("DU575569")
    .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response sample:

```json
{
    "code":0,
    "message": "success",
    "timestamp": 1530173419060,
    "data": {
        "orderId":105432
    }
}
```

### Place Order

Request Parameter：

 Parameter        | Type    | If required | Description                                                  
--- | --- | --- | ---
account       |string  |  Yes  |Trading Account ID:DU575569
symbol        |string	 |  Yes	 |Security Symbol e.g：AAPL
sec_type      |string	 |  Yes  |Contract Type (STK stock OPT US Futures WAR Hong Kong Warrant IOPT Hong Kong CBBC) 
action        |string	 |  Yes  |Transaction side BUY/SELL
order_type	  |string  |  Yes  | Order Type. MKT（Market order）, LMT（Limit order）, STP(Stop loss order), STP_LMT(Stop Limit), TRAIL(Tracking stop orders) 
total_quantity|int     |  Yes	 |Order Quantity(Hong Kong Stock，Shanghai-Hong Kong Stock Connect, Warship, CBBC has a minimum number of restrictions)
limit_price	  |double  |  No	 |Limit Price, this parameter is required when order_type is LMT, STP, STP_LMT
aux_price	  |double  |  No	 |Stock stop price. This parameter is required when order_type is STP, STP_LMT, and the tracking amount when order_type is TRAIL 
trailing_percent|double | No | Track stop-loss-percentage, when order_type is TRAIL, both aux_price and trailing_percent are mutually exclusive 
outside_rth   |boolean |  No   |True: Allows pre- and post-trade (US stock exclusive), false: not allowed, default allowed
market	      |string	 |  No	 |Market (US stocks, US, Hong Kong stocks, HK, Shanghai-Hong Kong, CN)
currency      |string  |	No	 |Currency (US Stock USD HK HKD Shanghai-Hong Kong Stock Connect CNH)
time_in_force |string  |  No  |Validity period can only be DAY (valid for the day) and GTC (valid before cancellation), default DAY
exchange      |string  |	No   |Exchange Stock Market (US stocks SMART HK stocks SEHK Shanghai-HK Stock Exchange SEHKNTL Shenzhen-HK Stock Exchange SEHKSZSE)
expiry	      |string	 |  No   |Expiration date (option, warrant, CBBC exclusive)
strike	      |string  |	No	 |Strike price (option, warrant, CBBC exclusive)
right         |string  |	No	 |Option direction PUT/CALL (option, warrant, CBBC exclusive)
multiplier    |float   |	No	 |Multiplier (option, warrant, CBBC exclusive)
local_symbol  |string  |    No   |the 5 digits below the Name under warrant, bull/bear  certificate list in the App (warrant, bull/bear certificate must fill this field)


Response Result：

 Name | Type | Description                                            
--- | --- | ---
id    | long | Unique ID, to search order/ change order/ cancel order 


Request Sample:
    
```json tab="US_STOCK"

{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-09-10 10:00:00",
  "method":"place_order",
  "biz_content":"{
      \"account\":\"DU575569\",
      \"order_id\":1234,
      \"symbol\": \"BIDU\",
      \"sec_type\": \"STK\",
      \"market\": \"US\",
      \"currency\": \"USD\",
      \"action\":\"BUY\",
      \"order_type\":\"LMT\",
      \"limit_price\": 9.0,
      \"total_quantity\": 10,
      \"time_in_force\": \"DAY\",
      \"outside_rth\": false
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

```json tab="US_OPT"

{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"1442394644440",
  "method":"place_order",
  "biz_content":"{
      \"account\":\"DU575569\",
      \"order_id\":1234,
      \"symbol\": \"BIDU\",
      \"sec_type\": \"OPT\",
      \"market\": \"US\",
      \"currency\": \"USD\",
      \"expiry\": \"20150821\",
      \"strike": \"160\",
      \"right": \"CALL\",
      \"multiplier\" : 100,
      \"action\":\"BUY\",
      \"order_type\":\"LMT\",
      \"limit_price\": 9.0,
      \"total_quantity\": 10,
      \"time_in_force\": \"DAY\",
      \"outside_rth\": false
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

```json tab="HK_STOCK"

{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"1442394644440",
  "method":"place_order",
  "biz_content":"{
      \"account\":\"DU575569\",
      \"order_id\":1234,
      \"symbol\": \"00700\",
      \"sec_type\": \"STK\",
      \"market\": \"HK\",
      \"currency\": \"HKD\",
      \"action\": \"BUY\",
      \"order_type\": \"LMT\",
      \"limit_price\": 9.0,
      \"total_quantity\": 10,
      \"time_in_force\": \"DAY\"
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

```json tab="WAR"

{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"1442394644440",
  "method":"place_order",
  "biz_content":"{
    \"account\":\"DU575569\",
    \"order_id\":1234,
    \"symbol\": \"2823\",
    \"sec_type\": \"WAR\",
    \"market\": \"HK\",
    \"currency\": \"HKD\",
    \"expiry\": \"20161202\",
    \"strike\": \"16.5\",
    \"right\": \"CALL\",
    \"multiplier\": 0.01,
    \"local_symbol\":13768,
    \"action\": \"BUY\",
    \"order_type\": \"LMT\",
    \"limit_price\": 9.0,
    \"total_quantity\": 1000,
    \"time_in_force\": \"DAY\"
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

```json tab="CHINA_STOCK"

{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"1442394644440",
  "method":"place_order",
  "biz_content":"{
    \"account\":\"DU575569\",
    \"order_id\":1234,
    \"symbol\": \"600016\",
    \"sec_type\": \"STK\",
    \"market\": \"CN\",
    \"currency\": \"CNH\",
    \"action\": \"BUY\",
    \"order_type\": \"LMT\",
    \"limit_price\": 9.0,
    \"total_quantity\": 10,
    \"time_in_force\": \"DAY\",
    \"outside_rth\": false
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

```json tab="FOREX"
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"1442394644440",
  "method":"place_order",
  "biz_content":"{
    \"account\":\"DU575569\",
    \"order_id\":1234,
    \"symbol\": \"USD\",        //required
    \"sec_type\": \"CASH\",     //required
    \"currency\": \"HKD\",      //required
    \"exchange\": \"IDEALPRO\", //required
    \"action\\": \"BUY\",
    \"order_type\": \"LMT\",
    \"limit_price\": 9.0,
    \"total_quantity\": 10,
    \"time_in_force\": \"DAY\",
    \"outside_rth\": false //只有false
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

```json tab="FUTURE"

{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"1442394644440",
  "method":"place_order",
  "biz_content":"{
      \"account\":\"DU575569\",
      \"order_id\":1234,
      \"symbol\": \"CN\",
      \"sec_type\": \"FUT\",
      \"market\": \"US\",
      \"currency\": \"USD\",
      \"expiry\": \"20190328\",
      \"multiplier\" : 1.0,
      \"action\":\"BUY\",
      \"order_type\":\"LMT\",
      \"limit_price\": 9.0,
      \"total_quantity\": 10,
      \"time_in_force\": \"DAY\",
      \"outside_rth\": false
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

 String bizContent = TradeParamBuilder.instance()
        .account("DU575569")
        .orderId(getOrderNo())
        .symbol("CN")
        .secType(SecType.FUT)
        .currency(Currency.USD)
        .action(ActionType.BUY)
        .orderType(OrderType.LMT)
        .expiry("20190328")
        .multiplier(1.0000000000F)
        .exchange("SGX")
        .limitPrice(9.0)
        .totalQuantity(10)
        .timeInForce(TimeInForce.DAY)
        .outsideRth(false)
        .buildJson();

SDK Request Sample:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey, publicKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.PLACE_ORDER);
int orderId = getOrderNo(); //You need to get the order number when ordering, to prevent duplicate orders

String bizContent = TradeParamBuilder.instance()
    .account("DU575569")
    .orderId(orderId)
    .symbol("AAPL")
    .totalQuantity(500)
    .limitPrice(61.0)
    .orderType(OrderType.LMT)
    .action(ActionType.BUY)
    .secType(SecType.STK)
    .currency(Currency.USD)
    .outsideRth(false)
    .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response Result:

```json
{
    "code": 0,
    "message": null,
    "timestamp": 1525937876720,
    "data": {
        "id":147070683398615040
    }
}
```

### Cancel Order

Request Parameter：

 Parameter | Type   | If required | Description                                    
--- | --- | --- | ---
account   | string  |  Yes  |Trading Account ID:DU575569
id  | long    |  Yes  |Order Number, created ID when placing an order

Response Result：

 Name | Type | Description                                                  
--- | --- | ---
id    | long | The unique number  used to query orders/modify orders/cancel orders 


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
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey, publicKey);
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

### Modify Order

Request Parameter:

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading Account ID:DU575569
id            |long    |  Yes  |Order id, return when placing an order
total_quantity|int     |  No  |Order Quantity(Hong Kong Stock，Shanghai-Hong Kong Stock Connect, Warship, CBBC has a minimum number of restrictions)
order_type	  |string  |  No  |Order Type. MKT（Market order）, LMT（Limit order）, STP(Stop loss order), STP_LMT(Stop Limit), TRAIL(Tracking stop orders)
limit_price	  |double  |  No  |Limit Price, this parameter is required when order_type is LMT, STP, STP_LMT
aux_price	  |double  |  No  |Stock stop price. This parameter is required when order_type is STP, STP_LMT, and the tracking amount when order_type is TRAIL
time_in_force |string  |  No  |Validity period can only be DAY (valid for the day) and GTC (valid before cancellation), default DAY


Response Result：

Name | Type | Description 
--- | --- | ---
id    | long | Unique ID, used to query orders/modify orders/cancel orders 

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
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey, publicKey);
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

