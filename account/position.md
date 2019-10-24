### Get Position

Request Parameter：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  	|  Yes  |Trading account id,DU575569
sec_type      |string  	|  No   |Security Type，include：STK/OPT/FUT， default STK
currency      |string  |  No   |Currency Type，include：ALL/USD/HKD/CNH default ALL
market        |string  |  No   |Market Type，include：ALL/US/HK/CN default ALL
symbol        |string  |  No  |Security symbol e.g：'SNAP' for stock, 'CL1909' for futures and 'AAPL  190111C00095000' for options.

Response Result：

Name | Type | Description 
--- | --- | ---
account|string|Trading account id
position|int|Position volume
averageCost|double|Average Cost
marketPrice|double|Market Price
marketValue|double|Market Value
realizedPnl|double|realized profit/loss
unrealizedPnl|double|unrealized profit/loss
salable|double|Number of shares tradable, used for China A shares only
currency|string|Transaction currency
expiry|string|Option expiry date
multiplier|double|number of shares per lot
right|string|option side
secType|string|Security Type
strike|double|Option strike price
symbol|string|Security Symbol

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"positions",
  "biz_content":"{\"account\":\"DU575569\",\"sec_type\": \"STK\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.POSITIONS);

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
	"data": {
		"items": [{
				"account": "DU575569",
				"averageCost": 111.3035,
				"contractId": 4347,
				"currency": "USD",
				"latestPrice": 91.83,
				"localSymbol": "ALB",
				"marketValue": 91.83,
				"multiplier": 0.0,
				"position": 1,
				"preClose": 0.0,
				"realizedPnl": 0.0,
				"secType": "STK",
				"status": 0,
				"stockId": "ALB.US",
				"symbol": "ALB",
				"unrealizedPnl": -18.97
			},
			{
				"account": "DU575569",
				"averageCost": 161.8235,
				"contractId": 6497,
				"currency": "USD",
				"latestPrice": 170.78,
				"localSymbol": "MCO",
				"marketValue": 170.78,
				"multiplier": 0.0,
				"position": 1,
				"preClose": 0.0,
				"realizedPnl": 0.0,
				"secType": "STK",
				"status": 0,
				"stockId": "MCO.US",
				"symbol": "MCO",
				"unrealizedPnl": 0.83
			}
		]
	},
	"timestamp": 1527830042620
}
```
