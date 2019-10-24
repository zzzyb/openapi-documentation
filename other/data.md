### API Instruction
The data interface mainly provides the query service of financial statement related data.

### Valuation data items updated on a daily basis

Request Parameters：

Parameter | Tyoe | If required | Description 
--- | --- | --- | ---
symbols       |array  	|  Yes  |Query stock symbol list，limit：50
market      |string  	|  Yes   |Query market，Support：US,CN,HK
fields        |string  |  Yes   |Query field name，reference：[Data dictionary/valuation indicators](../dict/valuation.md)，limit：10
begin_date |date  |  Yes  |Query start time，format：'2018-12-01'， TimeZone：UTC+8
end_date  |date|Yes   |Query end data，format：'2018-12-21'，TimeZone：UTC+8


Response Result：

Name | Type | Descripton 
--- | --- | ---
symbol|string|Stock symbol
date|double|Date
field|string|Field name
value|double|The field values


Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"financial_daily",
  "biz_content":"{\"symbols\":[\"AAPL\"],\"fields\": [\"sharesoutstanding\"],\"begin_date\":\"2018-10-01\",\"end_date\":\"2018-12-21\"}",  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample :
```java
List<String> symbols = new ArrayList<>();
symbols.add("AAPL");

List<String> fields = new ArrayList<>();
fields.add(FinancialDailyField.sharesoutstanding.name());

FinancialDailyResponse response = client.execute(FinancialDailyRequest.newRequest(symbols, fields, "2018-12-01", "2019-01-01"));
System.out.println(Arrays.toString(response.getFinancialDailyItems().toArray()));
```

Response Sample:
```json
{
	"code": 0,
	"data": [{
		"date": 1546272000000,
		"field": "sharesoutstanding",
		"symbol": "AAPL",
		"value": 4745398000
	}, {
		"date": 1546358400000,
		"field": "sharesoutstanding",
		"symbol": "AAPL",
		"value": 4745398000
	}, {
		"date": 1546444800000,
		"field": "sharesoutstanding",
		"symbol": "AAPL",
		"value": 4745398000
	}, {
		"date": 1546531200000,
		"field": "sharesoutstanding",
		"symbol": "AAPL",
		"value": 4745398000
	}, {
		"date": 1546617600000,
		"field": "sharesoutstanding",
		"symbol": "AAPL",
		"value": 4745398000
	}],
	"message": "success",
	"sign": "bS+Ep56665it0tJhihCF97gr0Ggvz3gCsASAj38tRT2NVLQ4JP+qGlb30Xrpn2rTV/hvNDQtSE3LqmLg0B4u2KXn4+lVFku7OEWt6QplbRA82wvDvqpEA7KN/CIZ2rzWGw6aWEmAwH2s42qo0j0KCgD+AcUpTK/K6e6hNcjCZY0=",
	"timestamp": 1547801776769
}
```

### Latest Financial data through company filing

Request Parameter：

Parameter | Type | If required | Description 
--- | --- | --- | ---
symbols   |array  	|  Yes  |Query stock symbols list, limit: 50
market    |string  	|  Yes   |Query market, support：US,CN,HK
fields    |string   |  Yes   |Query field name, reference：[P&L](../dict/income.md)，[Balance Sheet](../dict/balance_sheet.md)，[Cash Flow Statement](../dict/cash_flow.md)，[Derivative Index](../dict/profitability.md)，limit：10
period_type |string |  Yes  |Use 'Annual' to query the latest annual report data, 'Quarterly' to query the latest quarterly data, "LTM" for the last 12 month data .


Response Result：

Name | Type | Description 
--- | --- | ---
symbol|string|Stock symbol
currency|string|Transaction currency USD/HKD/CNH
field|string|Field name
value|string|Field value
filingDate|date|Release date of the financial report
periodEndDate|date|The closing date of the financial cycle


Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"financial_report",
  "biz_content":"{\"symbols\":[\"AAPL\"],\"fields\": [\"sharesoutstanding\"],\"market\":\"US\",\"period_type\":\"Annual\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:
```java
List<String> symbols = new ArrayList<>();
symbols.add("AAPL");
symbols.add("GOOG");
List<String> fields = new ArrayList<>();
fields.add(FinancialReportField.acctRecv3yrAnnCagr.name());
fields.add(FinancialReportField.cfShare.name());

FinancialReportResponse response = client.execute(FinancialReportRequest.newRequest(symbols, Market.US, fields, FinancialPeriodType.Annual));
System.out.println(Arrays.toString(response.getFinancialReportItems().toArray()));
```

Response Sample:
```json
{
	"code": 0,
	"data": [{
		"currency": "USD",
		"field": "acctRecv3yrAnnCagr",
		"filingDate": "2018-11-05",
		"periodEndDate": "2018-09-29",
		"symbol": "AAPL",
		"value": "11.2288"
	}, {
		"currency": "USD",
		"field": "cfShare",
		"filingDate": "2018-11-05",
		"periodEndDate": "2018-09-29",
		"symbol": "AAPL",
		"value": "12.939681"
	}, {
		"currency": "USD",
		"field": "acctRecv3yrAnnCagr",
		"filingDate": "2018-02-06",
		"periodEndDate": "2017-12-31",
		"symbol": "GOOG",
		"value": "25.0223"
	}, {
		"currency": "USD",
		"field": "cfShare",
		"filingDate": "2018-02-06",
		"periodEndDate": "2017-12-31",
		"symbol": "GOOG",
		"value": "34.502764"
	}],
	"message": "success",
	"sign": "aKeQyZud5auw/GQGNsKa1frNGFHGWZFBhnFssJqCTddCw5f4GsvQmG/WWA4oYRCZkU0oC5i0QNpu8xBFeBLT6vxa4uQP0sFhpxstzeC0q/GArEn+5/r8yJ64W/Pvfcqe8myJ+UXSbX2/4HRyqVLo7bptaaS0X7ygMStAog0sXm0=",
	"timestamp": 1547801675223
}
```

### Stock Split 

Request Parameter：

 Parameter   | Type   | If required | Description                                     
--- | --- | --- | ---
symbols   |array  	|  Yes  |Query stock symbol list, limit: 50
market    |string  	|  Yes   |Query market, support：US,CN,HK
action_type|string  |Yes  |Type, fixed to：split
begin_date |date  |  Yes  |Query start time, Format：'2018-12-01'， TimeZone：UTC+8
end_date  |date|Yes   |Query end data, format：'2018-12-21'，TimeZone：UTC+8


Response Result：

 Name        | Type   | Descripton       
--- | --- | ---
id|long|data id
symbol|string|Stock symbol
market|string|Market type
exchange|string|Exchange
executeDate|date|Execution date of the stock split/ joint stock
actionType|enum|Company action type
fromFactor|double|Pre-action factor
toFactor|double|Factor after company action
ratio|double|Reward ratio
type|string|Type of split stock


Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"corporate_action",
  "biz_content":"{\"symbols\":[\"UVXY\"],\"market\":\"US\",\"action_type\":\"split\",\"begin_date\":\"2018-01-01\",\"end_date\":\"2018-03-01\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample :
```java
List<String> symbols = new ArrayList<>();
symbols.add("UVXY");

CorporateSplitResponse response = client.execute(CorporateSplitRequest.newRequest(symbols, Market.US,
    DateUtils.getZoneDate("2008-01-01", TimeZoneId.Shanghai),
    DateUtils.getZoneDate("2019-02-20", TimeZoneId.Shanghai)));

System.out.println(JSONObject.toJSONString(response));
```

Response Sample:
```json
{
	"code": 0,
	"data": {
		"UVXY": [{
			"actionType": "SPLIT",
			"exchange": "ARCA",
			"executeDate": "2017-07-17",
			"fromFactor": 4.0,
			"id": 211,
			"market": "US",
			"ratio": 4.0,
			"symbol": "UVXY",
			"toFactor": 1.0
		}]
	},
	"message": "success",
	"success": true,
	"timestamp": 1552903119411
}
```

### Dividends

Request Parameter：

 Parameter   | Type   | If required | Description                                     
--- | --- | --- | ---
symbols   |array  	|  Yes  |Query stock symbol list, limit: 50
market    |string  	|  Yes   |Query market, support：US,CN,HK
action_type|string  |Yes  |Type, fixed to：dividend
begin_date |date  |  Yes  |Query start time, format：'2018-12-01'， TimeZone：UTC+8
end_date  |date|Yes   |Query end data, format：'2018-12-21'，TimeZone：UTC+8


Response Result：

 Name          | Type   | Descripton     
--- | --- | ---
id|long|Data id
symbol|string|Stock symbol
market|string|Market type
exchange|string|Stock exchange 
executeDate|date|Execution date of the stock split/ joint stock
actionType|enum|Company action type, in this case, "DIVIDEND"
recordDate|date|Registration Date	
announcedDate|date|Announcement date	
payDate|date|Payment date	
amount|double|Dividend amount	
currency|string|Dividend currency	


Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"corporate_action",
  "biz_content":"{\"symbols\":[\"AAPL\"],\"market\":\"US\",\"action_type\":\"dividend\",\"begin_date\":\"2018-01-01\",\"end_date\":\"2018-03-01\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:
```java
List<String> symbols = new ArrayList<>();
symbols.add("AAPL");

CorporateDividendRequest request = CorporateDividendRequest.newRequest(symbols, Market.US,
    DateUtils.getZoneDate("2019-01-01", TimeZoneId.Shanghai),
    DateUtils.getZoneDate("2019-02-20", TimeZoneId.Shanghai));
System.out.println(JSONObject.toJSONString(request));
CorporateDividendResponse response = client.execute(request);
  
System.out.println(JSONObject.toJSONString(response));
```

Response Sample:
```json
{
	"code": 0,
	"data": {
		"AAPL": [{
			"actionType": "DIVIDEND",
			"amount": 0.73,
			"announcedDate": "2019-01-29",
			"currency": "USD",
			"exchange": "NASDAQ",
			"executeDate": "2019-02-08",
			"id": 45240,
			"market": "US",
			"payDate": "2019-02-14",
			"recordDate": "2019-02-11",
			"symbol": "AAPL",
			"type": "RCD"
		}]
	},
	"message": "success",
	"success": true,
	"timestamp": 1552903241696
}
```
