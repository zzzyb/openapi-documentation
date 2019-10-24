---
title:Account Asset
keywords: Tiger Securites、Tiger API、 Open API
description: Tiger Open API help document
---

### Get Assets

Request Parameters：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading Account ID:DU575569
segment       |boolean |  No   | Whether to return a categorized account information component by security type. Default False.
market_value   |boolean |  No   | Whether to return a categorized account information component by currencies, Default False.

Response Result：

Name | Example | Description 
--- | --- | ---
account|DU575569|Trading Account
capability|RegTMargin|Account Type，Margin:RegTMargin，Cash:Cash
netLiquidation|1233662.93|Net liquidation value
equityWithLoan|1233078.69|Include stock borrowing and lending (include assets with loan value). Securities Segment: cash value + Stock value， futures Segment: cash value - maintenance margin
initMarginReq|292046.91|Initial margin requirement
maintMarginReq|273170.84|Maintenance Margin requirement
availableFunds|941031.78|Available Funds (available for trading)，Calculated as: equity_with_loan - initial_margin_requirement
dayTradesRemaining|-1|number of times remaining for day trade，-1 means unlimited
excessLiquidity|960492.09|excess liquidity，use to measure the risk of day trade. Calculation method for Securities Segment: equity_with_loan - maintenance_margin_requirement. Calculation for futures Segment: net_liquidation - maintenance_margin_requirement
buyingPower|6273545.18|The maximum amount of equity available to buy securities. In a Margin account, Buying Power gives you additional leverage to make trades, increasing your potential gain but also increasing your risk. Margin account gives you access to up to four times the excess liquidity intraday. Overnight buying power is up to two times of your excess liquidity.
cashValue|469140.39|Securities account cash value + futures account cash value
accruedCash|-763.2|Accrued interest for the current month, updated daily
accruedDividend|0.0|Accrued dividends. Refers to all accumulated dividends that have been announced but not yet paid.
grossPositionValue|865644.18|Total value of securities: long stock value + short stock value + long option value + short option value. Use absolute value for calculation
SMA|0.0|SMA (Special Memorandum Account) is a line of credit created when the market value of securities in a Margin account increases in value and maintained for the purpose of applying Federal Regulation T initial margin requirements at the end of the trading day. If the SMA balance at the end of the trading day is negative, your account is subject to liquidation.
regTEquity|0.0|For Securities Segment only，calculated equity with loan value based on Regulation T
regTMargin|0.0|For Securities Segment only, calculated initial margin requirements based on Regulation T
cushion|0.778569|excess-liquidity-to-total-assets ratio, calculated as : excess_liquidity/net_liquidation
currency|USD|Currency Type
realizedPnl|-248.72|realized gain or loss
unrealizedPnl|-17039.09|unrealized gain or loss
updateTime|0|data update time
**segments**||Categorize account information based on transaction security type. One dict has two keys, 'S' represents stocks, 'C' represents futures; value is an object of account.
**marketValues**||market value categorized by market. in one dict, 'USD' means the U.S. market, 'HKD' means the Hong Kong market; value is an object of the MarketValue

**Segments Description：**

Name | Example | Description 
--- | --- | ---
account|DU575569|Trading Account
category|S |Classification of underlying securities: "C" for US Commodities futures or "S" for US Securities
title|Title|Title
netLiquidation|1233662.93|Net liquidation value
cashValue|469140.39|cash value in the securities account+ cash value securities in the futures account
availableFunds|941031.78|Cash Available (for trading)
equityWithLoan|1233078.69|Equity with loan
excessLiquidity|960492.09|This is your margin cushion.For securities, this is equal to Equity with Loan Value – Maintenance Margin.For commodities, this Net Liquidation Value – Maintenance Margin.
accruedCash|-763.2|Accrued Cash
accruedDividend|0.0|Accrued Dividend
initMarginReq|292046.91|Initial margin requirement
maintMarginReq|273170.84|Maintenance margin requirement
regTEquity|0.0|RegT Equity
regTMargin|0.0|RegT Margin
SMA|0.0|Special Memorandum account, overnight risk index（App）
grossPositionValue|865644.18|gross Position Market Value
leverage|1|Leverage
updateTime|1526368181000|data updated time

**Market Values Descripion ：**

Name | Example | Description 
--- | --- | ---
account|DU575569|Trading Account ID
currency|USD|Currency Type
netLiquidation|1233662.93|Total Assets(net liquidation value)
cashBalance|469140.39|Cash
exchangeRate|0.1273896|exchange rate to the base currency of account
netDividend|0.0|net of dividend payable and dividend receivable
futuresPnl|0.0|Day end  profit/loss relative to last trading day
realizedPnl|-248.72|realized profit or loss
unrealizedPnl|-17039.09|unrealized profit or loss
updateTime|1526368181000|Update Time
stockMarketValue|943588.78|Stock Market Value
optionMarketValue|0.0|Option Market Value
futureOptionValue|0.0|Future Option market Value
warrantValue|10958.0|Warrant market Value

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"assets",
  "biz_content":"{\"account\":\"DU575569\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK  Request Sample:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.ASSETS);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
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
			"accruedCash": -763.2,
			"accruedDividend": 0.0,
			"availableFunds": 941031.78,
			"buyingPower": 6273545.18,
			"capability": "Reg T Margin",
			"cashBalance": 469140.39,
			"cashValue": 469140.39,
			"currency": "USD",
			"cushion": 0.778569,
			"dayTradesRemaining": -1,
			"equityWithLoan": 1233078.69,
			"excessLiquidity": 960492.09,
			"grossPositionValue": 865644.18,
			"initMarginReq": 292046.91,
			"maintMarginReq": 273170.84,
			"netLiquidation": 1233662.93,
			"netLiquidationUncertainty": 583.55,
			"previousEquityWithLoanValue": 1216291.68,
			"previousNetLiquidation": 1233648.34,
			"realizedPnl": -31.68,
			"unrealizedPnl": 1814.01,
			"regTEquity": 0.0,
			"regTMargin": 0.0,
			"SMA": 0.0,
		    "segments": [{
                "account": "DU575569",
                "accruedDividend": 0.0,
                "availableFunds": 65.55,
                "cashValue": 65.55,
                "category": "S",
                "equityWithLoan": 958.59,
                "excessLiquidity": 65.55,
                "grossPositionValue": 893.04,
                "initMarginReq": 893.04,
                "leverage": 0.93,
                "maintMarginReq": 893.04,
                "netLiquidation": 958.59,
                "previousDayEquityWithLoan": 969.15,
                "regTEquity": 958.59,
                "regTMargin": 446.52,
                "sMA": 2172.47,
                "title": "US Securities",
                "tradingType": "STKMRGN",
                "updateTime": 1541124813
            }],
           "marketValues": [{
                "account": "DU575569",
                "accruedCash": 0.0,
                "cashBalance": -943206.03,
                "currency": "HKD",
                "exchangeRate": 0.1273896,
                "futureOptionValue": 0.0,
                "futuresPnl": 0.0,
                "netDividend": 0.0,
                "netLiquidation": 11223.29,
                "optionMarketValue": 0.0,
                "realizedPnl": -248.72,
                "stockMarketValue": 943588.78,
                "unrealizedPnl": -17039.09,
                "updateTime": 1526368181000,
                "warrantValue": 10958.0
            },{
                "account": "DU575569",
                "accruedCash": 0.0,
                "cashBalance": -1635.23,
                "currency": "GBP",
                "exchangeRate": 1.35566495,
                "futureOptionValue": 0.0,
                "futuresPnl": 0.0,
                "netDividend": 0.0,
                "netLiquidation": 170.39,
                "optionMarketValue": 0.0,
                "realizedPnl": 0.0,
                "stockMarketValue": 1805.62,
                "unrealizedPnl": 177.58,
                "updateTime": 1526368181000,
                "warrantValue": 0.0
            },{
                "account": "DU575569",
                "accruedCash": 0.0,
                "cashBalance": 703542.12,
                "currency": "USD",
                "exchangeRate": 1.0,
                "futureOptionValue": 0.0,
                "futuresPnl": 0.0,
                "netDividend": 0.0,
                "netLiquidation": 1208880.15,
                "optionMarketValue": -64.18,
                "realizedPnl": 0.0,
                "stockMarketValue": 505780.03,
                "unrealizedPnl": 19886.87,
                "updateTime": 1526359227000,
                "warrantValue": 0.0
            }, {
                "account": "DU575569",
                "accruedCash": 0.0,
                "cashBalance": -714823.64,
                "currency": "CNH",
                "exchangeRate": 0.1576904,
                "futureOptionValue": 0.0,
                "futuresPnl": 0.0,
                "netDividend": 0.0,
                "netLiquidation": 142250.72,
                "optionMarketValue": 0.0,
                "realizedPnl": 0.0,
                "stockMarketValue": 859152.75,
                "unrealizedPnl": -102371.43,
                "updateTime": 1526368181000,
                "warrantValue": 0.0
           }]
	},
	"timestamp": 1527830042620
}
```
