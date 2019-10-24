### Get Sepcific Order Information

Request Parameter：

Parameter | Type | If Required | Description 
--- | --- | --- | ---
account  | string |  Yes   | Authorized user account:DU575569 
id |  int |  Yes   | Returned ID after successful place order 

Response Result：

Name | Example | Description 
--- | --- | ---
id|135482687464472583|Global unique order number
orderId|1000003917|User's self-incrementing number, non-globally unique
parentId|0|The order code of the master order for automatic tracking of stop orders
account|DU575569|Trading Account
action|BUY|transaction side, BUY or SELL
orderType|LMT|Order Type
limitPrice|36.91|Limit Price
auxPrice|0.0|Stop Loss Order Auxiliary Price - Trail Price
trailingPercent|5|Trace percentage of trailing stop orders
totalQuantity|50|Total Quantity
timeInForce|DAY|DAY/GTC
outsideRth|false|whether allow pre and post-trade
filledQuantity|100|Filled Quantity
lastFillPrice|0.0|last Filled Price
avgFillPrice|12.0|Average filling price including commission
liquidation|false|If liquidation
remark|incorrect order number|Error message
status|Filled|Order Status, reference:[Order Status](../intro/model.html#_3)
commission|0.99|Includes commissions, stamp duty, CSRC fees, etc.
commissionCurrency|USD|Commission Currency
realizedPnl|0.0|Realized profit or loss
percentOffset|0.0|Percentage offset for order
openTime|2019-01-01|Place Order Time
tradeTime|2019-01-01|Latest Transaction Time
latestTime|2019-01-01|Latest Update Status Time
symbol|JD|Stock symbol
currency|USD|Currency
market|US|Trading Market
multiplier|0.0|number of shares per lot
secType|STK|Transaction Type

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"orders",
  "biz_content":"{\"account\":\"DU575569\",\"id\": 147070683398615040}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK request sample:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.ORDERS);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
        .id(147070683398615040L)
        .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);

```

Return Sample：
```json
{
    "code": 0,
    "message": "success",
    "data": {
      "id":135482687464472583,//Global unique order ID
      "account": "DU575569", //trading account
      "action": "BUY", //
      "auxPrice": 0.0, //stop order auxiliary price - trail price
      "avgFillPrice": 0.0, //avegrage filled price
      "currency": "USD",
      "filledQuantity": 0, //filled quantity
      "lastFillPrice": 0.0, //last filled price
      "limitPrice": 36.91, //limit price
      "market": "US",
      "multiplier": 0.0,
      "orderId": 1000003917,//user's order autoincrement ID, global not unique
      "orderType": "LMT", 
      "outsideRth": false, //whether allow pre and post-trade
      "realizedPnl": 0.0, //realized profit and loss
      "secType": "STK",
      "status": "Filled",
      "symbol": "JD",
      "timeInForce": "DAY", //DAY/GTC
      "totalQuantity": 50 // 
    },
    "timestamp": 1527830042620
}
```


### Get Orders

Request Parameter：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes | Trading account ID:DU575569 
sec_type      |string  |  No  | ALL/STK/OPT/FUT/FOP/CASH default ALL 
market        |string  |  No  | ALL/US/HK/CN default ALL 
symbol        |string  |  No  | Stock symbol 
start_date    |string  |  No  | '2018-05-01' or "2018-05-01 10:00:00" 
end_date      |string  |  No  | '2018-05-15' or "2018-05-01 10:00:00" 
states        |array   |  No  | Search order default，Order Status, reference:[Order Status](../intro/model.html#_3) 
limit         |integer |  No  | Default：100, Max limit: 300 


Response Result：

Refer to the get order interface

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"orders",
  "biz_content":"{\"account\":\"DU575569\",\"sec_type\": \"STK\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.ORDERS);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
        .secType(SecType.STK)
        .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);

```

Response Sample:
```json
{
    "code": 0,
    "message": "success",
    "data":{
        "items":[{
          "account": "DU575569", //trading account
          "id":135482687464472583,//Global unique order ID
          "action": "BUY", //
          "auxPrice": 0.0, //Stop loss order auciliary price- Trail Price
          "avgFillPrice": 0.0, //Buying price
          "currency": "USD",
          "filledQuantity": 0, // filled quantity
          "lastFillPrice": 0.0, //last filled price
          "limitPrice": 36.91, //limit price
          "market": "US",
          "multiplier": 0.0,
          "orderId": 1000003917,
          "orderType": "LMT", 
          "outsideRth": false, // whether allow pre and post-trade
          "realizedPnl": 0.0, //Actual profit and loss
          "secType": "STK",
          "status":3,
          "symbol": "JD",
          "timeInForce": "DAY", //DAY/GTC
          "totalQuantity": 50 // total quantity
        }, {
          "account": "DU575569",
          "id":135482687464472583,//Global unique order ID
          "action": "BUY",
          "auxPrice": 0.0,
          "avgFillPrice": 0.0,
          "currency": "USD",
          "filledQuantity": 0,
          "lastFillPrice": 0.0,
          "limitPrice": 19.21,
          "market": "US",
          "multiplier": 0.0,
          "orderId": 1000003874,
          "orderType": "LMT",
          "outsideRth": true,
          "realizedPnl": 0.0,
          "secType": "STK",
          "symbol": "JKS",
          "timeInForce": "DAY",
          "totalQuantity": 4858,
          "trailStopPrice": 0.0, //Trail stop price
          "trailingPercent": 0.0 
        }]
    },
 	"timestamp": 1527830042620
}
```

### Filled Order List

Request Parameters：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading account ID:DU575569
symbol        |string  |  No  |Security symbol
sec_type      |string  |  No   |ALL/STK/OPT/FUT/FOP/CASH default ALL
start_date    |string  |  No  |'2018-05-01' or "2018-05-01 10:00:00"
end_date      |string  |  No  |'2018-05-15' or "2018-05-01 10:00:00"

Response Result：

Refer to the "Get Orders" API above.

Request Sample:

```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"filled_orders",
  "biz_content":"{\"account\":\"DU575569\",\"sec_type\": \"STK\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.FILLED_ORDERS);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
        .secType(SecType.STK)
        .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response Result:

Refer to the get order interface

### Pending Order List

Request Parameters：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Authorized user account:DU575569
symbol        |string  |  No  |Stock Symbol
sec_type      |string  |  No   |ALL/STK/OPT/FUT/FOP/CASH default ALL
start_date    |string  |  No  |'2018-05-01' or "2018-05-01 10:00:00"
end_date      |string  |  No  |'2018-05-15' or "2018-05-01 10:00:00"

Response Result：

Refer to the "Get Orders" API above.

Request Sample:

```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"active_orders",
  "biz_content":"{\"account\":\"DU575569\",\"sec_type\": \"STK\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK request sample:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.ACTIVE_ORDERS);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
        .secType(SecType.STK)
        .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response result:

Refer to the get order interface


### Canceled Order List

Request parameters：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading Account ID:DU575569
symbol        |string  |  No  |Security Symbol
sec_type      |string  |  No   |ALL/STK/OPT/FUT/FOP/CASH default ALL
start_date    |string  |  No  |'2018-05-01' or "2018-05-01 10:00:00"
end_date      |string  |  No  |'2018-05-15' or "2018-05-01 10:00:00"

Response Result：

Refer to the "Get Orders" API above.

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"inactive_orders",
  "biz_content":"{\"account\":\"DU575569\",\"sec_type\": \"STK\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.INACTIVE_ORDERS);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
        .secType(SecType.STK)
        .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Return Request:

Refer to the get order interface
