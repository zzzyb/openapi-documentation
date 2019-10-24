## Request Address

Environment | Request adress |Content-Type| Request  Method 
--- | --- | --- | ---
Formal environment | https://openapi.itiger.com/gateway | application/json | POST
Test environment (apply separately) | https://openapi-sandbox.itiger.com/gateway | application/json | POST

## Public Request Parameter

Parameter | Type | If required | Max length | Description | Sample value 
--- | --- | --- | --- | --- | ---
tiger_id    | string | yes | 10   | ID assigned to the developer | 20150129
method      | string | yes | 128  | API name, see:[method instructions](#method) | place_order
charset     | string | yes | 10   | Please use coding format, currently supports ：UTF-8 | UTF-8
sign_type   | string | yes | 10   | The signature algorithm type used by the merchant to generate the signature string, currently supports RSA | RSA
timestamp   | string | yes | 19   | Request time, format "yyyy-mm-dd HH:mm:ss"	| 2018-05-09 11:30:00
version     | string | yes | 3    | API version, current version supports：1.0, 2.0.Please check the socket documentation for specifics | 1.0
biz_content | string | yes | -    | The collection of request parameters, all business parameters except public parameters are passed in this parameter, refer to the interface access document for details |
sign        | string | yes | 344  | For the signature string of the request parameter, see: [signature rule](./sign.md)	| See example 
account_type| string | no | -    | Account type, token authentication must pass, including the type:[GLOBAL,STANDARD] |
access_token| string | no | -    | Token authentication must pass, used to verify the identity of the user, can be obtained through the login interface |
trade_token | string | no | -    | Token (optional when authentication), the order related interface must pass, can be obtained through the transaction password interface |


## Public Respoonse Parameter

Parameter | Type | If required | Description | Sample value 
--- | --- | --- | --- | ---
code            | string | yes | Return code,: [code field description](#code) | 40001
message         | string | Yes | Return code description, see: code field description | post param is empty
data            | string | no | Business returns content, see the specific API interface documentation for details. | -
timestamp       | string | Yes | Timestamp returned by the gateway | 1525849042762



## Request Sample
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-09-01 10:00:00",
  "method":"order_no",
  "biz_content":"{\"account\":\"DU575569\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

## Response Sample
```json
{
    "code": 0,
    "message": "",
    "timestamp": 1525849042762,
    "data": {
        "orderId":10000164
    }
}
```

## Callback Interface

If the callback URL is provided when accessing the system, the callback address will be requested when there is a push message. The message format of the callback interface is:

```json
{
    "subject":"Asset",
    "data":{
        "type":"asset",
        "usdCash":854879.28,
        "account":"DU575567",
        "timestamp":1528719139003
    }
}
```

### Callback Type

* subject type:[Subject](/docs/api/struct#subject)

### Field Change

* Order related field:[Order change](/docs/api/struct#orderChange)
* Asset related Field:[Asset change](/docs/api/struct#assetChange)
* Position related fields:[Position change](/docs/api/struct#positionChange)


## <span id="method">Method Instructions</span>

method|Description
--- | ---
order_no             |Order Number
place_order          |Place order
cancel_order         |Cancel order
modify_order         |Modify order
market_state         |Market state   
all_symbols          |Obtain Stock Symbol
all_symbol_names     |Obtain Stock Symbol and Name
brief                |Get stock abstract
stock_detail         |Get stock details
timeline             |Acquire time-sharing data
hour_trading_timeline|Get time-sharing data before stock market opened/ after stock market closed
kline                |Kline data
trade_tick           |Trade tick data
contract             |Contract data
assets               |Assets
positions            |Positions
orders               |Order list
filled_orders        |Filled orders list
active_orders        |Active orders list
inactive_orders      |nactive orders list
quote_contract       |Quote contract list
quote_real_time      |Real time quote
quote_shortable_stocks|Shortable stocks quote
quote_stock_trade    |Stock trade quote
financial_daily      |Daily financial data
financial_report     |Financial report data
option_expiration    |Option expiration date
option_chain         |Option chain
option_brief         |Option Quote
option_kline         |Option Kline
option_trade_tick    |Option trade tick
future_exchange      |Future exchange list
future_contract_by_contract_code|Futures contract details
future_contract_by_exchange_code|Tradeable contract under the exchange
future_continuous_contracts|Future continuous contracts
future_current_contract|Future current contract
future_kline|Future kline
future_real_time_quote|Future real time quote
future_tick|Future tick data
future_trading_date|Future trading date

## <span id="code">Code instructions</span>

code|Description
--- | ---
0     | Request successful 
1     | Server error 
2     | Network timeout 
3     | Client error 
10000 | General error 
20001 | Interface error 
20002 | Unable to find account 
20003 | Empty order or can not find order through ordered and account 
20004 | Order does not match 
20005 | Verify order exception, server does not return 
20006 | Account do not match 
20007 | Not logged in to trading account 
20008 | Duplicate order Id，orderId already exists or orderId not in the list of generated Ids 
20009 | account can't find the corresponding Gateway(account not valid or gateway disconnected) 
20010 | Verify order exception 
20011 | Order is not a valid order (A valid order means that the order status is PreSubmitted, PendingCancel, Submitted, PendingSubmit) 
20012 | Verify order commission exception 
20013 | Unable to connect to the trading service, stop receiving order operations, usually weekend non-trading time 
20014 | Unopened Account 
20015 | Transaction access denied 
21001 | contract error 
21002 | symbol can not be empty 
21003 | quantity error 
21004 | price error 
21005 | symbol error 
21006 | contract error 
21007 | action error 
21008 | order type error 
30000 | json format error 
40000 | The required order field does not exist or is illegal 
40001 | Request parameter is empty 
40002 | Tiger_id is empty 
40003 | Tiger_id is not authorized 
40004 | Method is empty 
40005 | Method is not supported 
40006 | Timestamp format error 
40007 | charset not supported 
40008 | version not supported 
40009 | sign_type not supported 
40010 | biz_content error 
40011 | account can not be empty 
40012 | unauthorized account 
40013 | Verify signature error 
40014 | Not authorized IP address 
40015 | Access rights error 
40016 | Access frequency too fast 
40017 | The number of sub accounts exceeds the limit (maximum 1000) 
40018 | Sub-account is not authorized 
40019 | Account type is not supported 
40020 | Subscribe to tiger_id error 
40021 | Subscribe to symbol error 
40022 | Subscribe to symbol exceeds limit (maximum 500) 
40023 | Subscribe to subject error 
40024 | Unsubscribe failed (symbol/tiger_id error) 


## Limit Instructions

* Current limit unit: Requests per minute
* Current-limit type: IP current limit, account current limit, and interface current limit.
* High-frequency interface: market-related interface (except for the acquisition of stock code interface), create order interface, cancel order interface

IP (request/min)| TigerId (request/min) | Socket (request/min) |High frequency Socket（request/min）
---|---|---|---
600|600|60|120

