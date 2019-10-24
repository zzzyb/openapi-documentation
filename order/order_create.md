### Place Order 

Request Parameter：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading Account ID:DU575569
order_id      |int     |  No   |Local order id, used to prevent duplicate orders. Request through order number interface. If set to 0, the server will automatically generate a order number. Can not prevent duplicate orders. Please choose carefully.
symbol        |string	 |  Yes	 |Security symbol e.g：AAPL
sec_type      |string	 |  Yes  |Contract Type (STK stock OPT US Futures WAR Hong Kong Warrant IOPT Hong Kong CBBC) 
action        |string	 |  Yes  |Transaction side BUY/SELL
order_type	  |string  |  Yes  |Order Type. MKT（Market order）, LMT（Limit order）, STP(Stop loss order), STP_LMT(Stop Limit), TRAIL(Tracking stop orders)
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
strike	      |string  |	No	 |Bottom price (option, warrant, CBBC exclusive)
right         |string  |	No	 |Option side PUT/CALL (option, warrant, CBBC exclusive)
multiplier    |float   |	No	 |one lot unit (option, warrant, CBBC exclusive)
local_symbol  |string  |    No   |the 5 digits below the Name under warrant, bull/bear  certificate list in the App (warrant, bull/bear certificate must fill this field)


Response Result：

Name | Type | Description 
--- | --- | ---
id    | long | Unique ID, can be used to search order/ change order/ cancel order 


Request Sample:
    
#### US Stocks
```json

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

#### US Stock Options
```json

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

#### HongKong Stocks
```json

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

#### Warrant /CBBC
```json

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
    \"symbol\": \"002823\",
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

#### Shanghai and Shenzhen Stocks
```json

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

#### Foreign Currency Exchange
```json
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
    \"outside_rth\": false // only false
  }",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

#### Futures Order
```json 

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

 SDK Request Sample:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.PLACE_ORDER);
int orderId = getOrderNo(); //You need to get the order number before place order, to prevent duplicate orders

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
