---
title: Order Preview 
keywords: Tiger Securities、Tiger API、 Open API
description: Tiger Open API help document 
---

### Order Preview 

Request Parameter：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading Account ID:DU575569
symbol        |string	 |  Yes	 |Security symbol e.g：AAPL
sec_type      |string	 |  Yes  |Security Type (STK for Stock, OPT for U.S. Stock options, WAR for Hong Kong Stock Warrant, IOPT for Hong Kong Stock bull/bear certificate) 
action        |string	 |  Yes  |transaction side BUY/SELL
order_type	  |string  |  Yes  |Order Type. MKT (market order）, LMT (limit order）, STP (stop order), STP_LMT (stop limit order), TRAIL (trailing stop order)
total_quantity|int     |  Yes	 |Order Quantity (there is a minimum quantity limit for Hong Kong stocks, shanghai-hong kong stock connect, warrant and bull/bear certificate)
limit_price	  |double  |  No	 |Limit price, required when order_type is LMT,STP,STP_LMT
aux_price	  |double  |  No	 |Stop price of stock. This parameter is required when order_type is STP,STP_LMT. When order_type is TRAIL, it means trailing amount 
trailing_percent|double | No | Trailing stop orders - percentages, aux_price and trailing_percent are mutually exclusive when order_type is TRAIL 
outside_rth   |boolean |  No   |True: allow after-hours trading (the U.S. stock exclusive), false: not allowed, default allowed
market	      |string	 |  No	 |Market (the U.S. stock: US, Hong Kong stocks: HK, Shanghai-Hong Kong stock connect: CN)
currency      |string  |	No	 |Currency (the U.S. stock: USD, Hong Kong stocks: HKD, Shanghai-Hong Kong stock connect: CNH)
time_in_force |string  |  No  |The validity of the order can only be DAY (valid on the DAY) and GTC (valid before cancellation), the default is DAY
exchange      |string  |	No   |Exchange (the U.S. stock: SMART, Hong Kong stocks: SEHK, Shanghai-Hong Kong stock connect: SEHKNTL, Shenzhen-Hong Kong stock connect: SEHKSZSE)
expiry	      |string	 |  No   |Expiration date (exclusive for option, warrant and bull/bear certificate)
strike	      |string  |	No	 |strike price (exclusive for option, warrant and bull/bear certificate)
right         |string  |	No	 |option side PUT/CALL (exclusive for option, warrant and bull/bear certificate)
multiplier    |float   |	No	 |one lot unit (option, warrant, bull/bear certificate only)
local_symbol  |string  |    No   |the 5 digits below the Name under warrant, bull/bear  certificate list in the App (warrant, bull/bear certificate must fill this field)


Response Result：

Name | Type | Description 
--- | --- | ---
account|string|User Account
status|string|According to the order parameters, determine the order status may appear after placing the real order
initMargin|double|Initial margin amount, only margin accounts have this attribute
maintMargin|double|Maintenance margin amount, only margin accounts have this attribute
equityWithLoan|double|Asset value with borrowing value, for a cash account = cashBalance, for margin account = cashBalance + grossPositionValue
initMarginBefore|double|Initial Margin before placing order
maintMarginBefore|double|Maintenance margin before placing order
equityWithLoanBefore|double|Pre-order equity with loan value
marginCurrency|string|base currency of margin account
commission|double|Commission
minCommission|double|The minimum commission calculated when the commission cannot be confirmed
maxCommission|double|The maximum commission calculated when the commission cannot be confirmed
commissionCurrency|double|Commission currency, for example: the main currency of the account is Hong Kong dollar. When placing orders for us stocks, the commission currency is USD
warningText|string|Warning Text


Request Sample:
```json

{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-09-10 10:00:00",
  "method":"preview_order",
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

SDK Request Sample:

```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.PREVIEW_ORDER);

String bizContent = TradeParamBuilder.instance()
    .account("DU575569")
    .orderId(0)
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

Response Sample:

```json
{
    "code": 0,
    "message": null,
    "timestamp": 1525937876720,
    "data": {
	    "commission": 2.0,
	    "initMargin": 3000,
	    "maintMargin": 1000,
	    "equityWithLoan": 1500
    }
}
```

