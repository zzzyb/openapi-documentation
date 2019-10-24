### Get Contract information

Request Parameters：

Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Trading account ID:DU575569
symbol        |string  |  Yes  |Security Symbol e.g：CL1909 / SNAP
sec_type      |string  |  Yes  |STK/OPT default STK
currency      |string  |  No   |ALL/USD/HKD/CNH default ALL
expiry        |string  |  No   |Expiry date - when the symbol is an option, it must be provided
strike        |double  |  No   |Exercise price - when the symbol is an option, it must be provided
right         |string  |  No   |C/P - when the symbol is an option, it must be provided
exchange      |string  |  No   |Exchange (U.S. stocks SMART, HK stocks SEHK, Shanghai-Hongkong stock connect SEHKNTL, Shenzhen-Hongkong connect SEHKSZSE)


Response Result：

Name | sample | Description 
--- | --- | ---
contractId| 1 | IB contracts' code of the bond 
symbol| LRN | Stock Symbol 
secType|STK |STK/OPT default STK
name| K12 INC | Stock name 
localSymbol|1033 |Used to identify warrants and CBBCs in Hong Kong stocks.
currency|USD |USD/HKD/CNH
exchange|NYSE |Exchange 
primaryExchange|NYSE|Primary Market exchange
market| US | Market /US/HK/CN 
expiry| 20171117 | Option Expiration Date 
contractMonth|201804 |Month of Contract
right| PUT |side of a option
strike| 24.0|strike price of a option
multiplier| 0.0 |Multiplier of the future or option contract
minTick|0.001|Minimum quote unit
tradingClass|LRN |Transaction level of contract
status|1 |Status
continuous|false| Used to identifiy weather a future contract is continous
trade|false|can be traded or not, future contracts only.
lastTradingDate|2019-01-01|the last trading day of a future contract.
firstNoticeDate|2019-01-01|the first notice date, beyound which you can't open any positions. Current position will be forced to liquidate before the first notice day(usually three days before the first notice day)
lastBiddingCloseTime|0|Only futures, the auction deadline

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-05-01 10:00:00",
  "method":"contract",
  "biz_content":"{\"account\":\"DU575569\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample :
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.CONTRACT);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
        .symbol("AAPL")
        .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response sample:
```json
{
    "code": 0,
    "message": "success",
    "data":{
        "items":[{
            "contractId": 42451645,
            "currency": "HKD",
            "exchange": "SEHK",
            "localSymbol": "1058",
            "minTick": 0.001,
            "multiplier": 2000.0,
            "name": "GUANGDONG TANNERY LTD",
            "secType": "STK",
            "symbol": "01058",
            "tradingClass": "1058"
        }]
    }
}
```


### Request Warrants Contract List

Request Parameters： 

Parameter | Type | If required | Description 
--- | --- | --- | ---
symbols |array|Yes|List of security symbols, only support one symbol
sec_type|string	 |  Yes  | Type of contracts, now support:(WAR Warrants IOPT CBBCs-in-HK) 
lang|string|No|Language supported: zh_CN,zh_TW,en_US, default: en_US


Response Result： 

Name|Type|Description
--- | --- | ---
symbol|string|Stocks' symbol
name | string  | Contract name 
exchange | string  | Exchange 
market | string | Market 
secType | string  | Contract Type 
currency | string  | Currency 
expiry | string  | Expiry date(options, warrants, CBBCs, futures), 20171117 
right | string  | side of options(options, warrants, CBBCs),PUT/CALL 
strike | string  | strike price 
multiplier | double | Trading volume per lot(options, warrants, CBBCs, futures) 


Request Sample：
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"2.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"quote_contract",
  "biz_content":"{\"symbols\":[\"AAPL\",\"GOOG\"],\"sec_type\":\"WAR\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Request Sample:
```java
QuoteContractResponse response = client.execute(QuoteContractRequest.newRequest(List.of("00700"), SecType.WAR));
if (response.isSuccess()) {
  System.out.println(response.getContractItem());
} else {
  System.out.println("response error:" + response.getMessage());
}
```

Response Sample：
```json
{
  "code": 0,
  "data": {
    "symbol": "03690",
    "secType": "WAR",
    "items": [
      {
        "market": "HK",
        "symbol": "12415",
        "multiplier": 1000.0,
        "strike": "75.88",
        "name": "HSMTUAN@EC1904A.C",
        "currency": "HKD",
        "exchange": "SEHK",
        "secType": "WAR",
        "right": "CALL",
        "expiry": "2019-04-03"
      },
      {
        "market": "HK",
        "symbol": "16083",
        "multiplier": 100.0,
        "strike": "69.08",
        "name": "BPMTUAN@EC1903A.C",
        "currency": "HKD",
        "exchange": "SEHK",
        "secType": "WAR",
        "right": "CALL",
        "expiry": "2019-03-12"
      }
    ]
  },
  "timestamp": 1544586701445,
  "message": "success"
}
```


### Request Futures Contract

Request Parameters： 

Parameter | Type | If required | Description 
--- | --- | --- | ---
contract_code|string|yes|Contract Symbol, e.g.: CN1901
lang|string|no|Language parameter,defines language used in the "name" field. Value range: "zh_CN", "en_US"


 Response Result： 

Name|Type|Description
--- | --- | ---
type|string|Futures contracts symbol, e.g. :CL
trade|boolean|Can be traded or not
continuous|boolean|Continuous or not
name|string|contact name, it has simplified Chinese or English, based on returning of parameter "lang"
currency|string|Trading Currency
ibCode|string|Trading Contract code, for placing order e.g.: CL
contractCode|string|Contract code, e.g.: CL1901
contractMonth|string|Contract delivery month
lastTradingDate|string|Last trading day
firstNoticeDate|string|First day notice, the contract cannot open long piston after the first notice day.  Existing long position will be forced to liquidate before the first notice day(usually before the first three  trading days)
lastBiddingCloseTime|long|Auction deadline

Request Sample：
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"future_contract_by_contract_code",
  "biz_content":"{\"contract_code\":\"CN1901\",\"lang\":\"zh_CN\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Request Sample:
```java
FutureContractResponse response = client.execute(FutureContractByConCodeRequest.newRequest("CN1901"));
System.out.println(response.getFutureContractItem());
```

Response Sample：
```json
{
	"code": 0,
	"serverTime": 1545049140384,
	"message": "success",
	"data": {
		"type": "CL",
		"trade": true,
		"continuous": false,
		"name": "WTICrudeoil1901",
		"currency": "USD",
        "ibCode":"CL",
  		"contractCode": "CL1901",
		"contractMonth": "201901",
		"firstNoticeDate": "20181221",
		"lastTradingDate": "20181219",
		"lastBiddingCloseTime": 0
	}
}
```

### Request Exchange Tradable Contract (Futures）

Request Parameter： 

Parameter | Type | If required | Description 
--- | --- | --- | ---
exchange_code|string|yes|Exchange code
lang|string|no|Language parameter,,defines language used in the "name" field. Value range: "zh_CN", "en_US"


Response Result： 

Name|Type|Description
--- | --- | ---
type|string|Futures contracts symbol, e.g. :CL
trade|boolean|Can be traded or not
continuous|boolean|Continuous or not
name|string|Contact name, it has simple Chinese or English, based on returning of parameter "lang"
currency|string|Currency
ibCode|string|Trading Contract code, for placing order e.g.: CL
contractCode|string|Contract code, e.g.: CL1901
contractMonth|string|Contract delivery month
lastTradingDate|string|Last trading day
 firstNoticeDate      | string  | First day notice, the contract cannot open long piston after the first notice day.  Existing long position will be forced to liquidate before the first notice day(usually before the first three  trading days) 
 lastBiddingCloseTime | long    | Auction deadline                                             

Request Sample ：
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"future_contract_by_exchange_code",
  "biz_content":"{\"exchange_code\":\"CME\",\"lang\":\"zh_CN\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Request Sample :
```java
FutureBatchContractResponse response = client.execute(FutureContractByExchCodeRequest.newRequest("CME"));
System.out.println(response.getFutureContractItems());
```

Response Sample：
```json
{
	"code": 0,
	"serverTime": 1545049140384,
	"message": "success",
	"data": [{
		"type": "CL",
		"trade": true,
		"continuous": false,
		"name": "WTIcrudeoil1901",
		"currency": "USD",
        "ibCode":"CL",
  		"contractCode": "CL1901",
		"contractMonth": "201901",
		"firstNoticeDate": "20181221",
		"lastTradingDate": "20181219",
		"lastBiddingCloseTime": 0
	}
```

### Search Type Continous Contract (futures)

Request Parameters： 

Parameter | Type | If required | Descripton 
--- | --- | --- | ---
type|string|yes|Futures Contract related to trade type, e.g CL
lang|string|yes|Language parameter, have an effect on the "name" field in the return value. Value range: "zh_CN", "en_US"

Response Result： 

Name|Type|Description
 --- | --- | ---
type|string|Futures contracts symbol, e.g. :CL
trade|boolean|Can be traded or not
continuous|boolean|Continuous or not
name|string|Contact name, it has simplfied Chinese or English, based on returning of parameter "lang"
currency|string|Currency
ibCode|string|Trading Contract code, for placing order e.g.: CL
contractCode|string|Contract code, e.g.: CL1901
contractMonth|string|Contract delivery month
lastTradingDate|string|Last trading day
firstNoticeDate|string|First day notice, the contract cannot open long piston after the first notice day.  Existing long position will be forced to liquidate before the first notice day(usually before the first three  trading days)
lastBiddingCloseTime|long|Bidding close time

Request Sample：
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"future_continuous_contracts",
  "biz_content":"{\"type\":\"CL\",\"lang\":\"zh_CN\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Sample Request:
```java
FutureContractResponse cl = client.execute(FutureContinuousContractRequest.newRequest("CL"));
System.out.println(cl.getFutureContractItem());
```

Response Sample：
```json
{
	"code": 0,
	"serverTime": 1545049140384,
	"message": "success",
	"data": {
		"type": "CL",
		"trade": true,
		"continuous": false,
		"name": "WTICrudeoil1901",
		"currency": "USD",
        "ibCode":"CL",
  		"contractCode": "CL1901",
		"contractMonth": "201901",
		"firstNoticeDate": "20181221",
		"lastTradingDate": "20181219",
		"lastBiddingCloseTime": 0
	}
}
```

### Search Specific Type Contract (futures)

Request Parameters： 

Parameter | Type | If required | Description 
--- | --- | --- | ---
type|string|yes|Futures contracts symbol, e.g. :CL
lang|string|no|Language parameter, have an effect on the "name" field in the return value. Value range: "zh_CN", "en_US"

Response Result： 

Name|Type|Description
 --- | --- | ---
type|string|Futures contracts symbol, e.g. :CL
trade|boolean|Can be traded or not
continuous|boolean|Continuous or not
name|string|Contact name, it has simple Chinese or English, based on returning of parameter "lang"
currency|string|Currency
ibCode|string|Trading Contract code, for placing order e.g.: CL
contractCode|string|Contract code, e.g.: CL1901
contractMonth|string|Contract delivery month
lastTradingDate|string|Last trading day
firstNoticeDate|string|First day notice, the contract cannot open long piston after the first notice day.  Existing long position will be forced to liquidate before the first notice day(usually before the first three  trading days)
lastBiddingCloseTime|long|Bidding close time

Request Sample :
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"future_current_contract",
  "biz_content":"{\"type\":\"CL\",\"lang\":\"zh_CN\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Request Sample :
```java
FutureContractResponse response = client.execute(FutureCurrentContractRequest.newRequest("CL"));
System.out.println(response.getFutureContractItem());
```

Response Sample:
```json
{
	"code": 0,
	"serverTime": 1545049140384,
	"message": "success",
	"data": {
		"type": "CL",
		"trade": true,
		"continuous": false,
		"name": "WTICrudeoil1901",
		"currency": "USD",
        "ibCode":"CL",
  		"contractCode": "CL1901",
		"contractMonth": "201901",
		"firstNoticeDate": "20181221",
		"lastTradingDate": "20181219",
		"lastBiddingCloseTime": 0
	}
}
```


### Search Specific Futures Contract Trade Time 

Request Parameters： 

Parameter | Type | If required | Description 
--- | --- | --- | ---
contract_code|string|yes|Contract code, e.g.: CL1901
trading_date|long|yes|Trading day timestamp

Response Result： 

Name|Type|Description
 --- | --- | ---
tradingTimes|array|Trading Time
biddingTimes|array|Bidding Time
timeSection|string|Trading Time zone

Request Sample：
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-12-01 10:00:00",
  "method":"future_trading_date",
  "biz_content":"{\"contract_code\":\"CL\",\"trading_date\":1545049282852}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

SDK Request Sample:
```java
FutureTradingDateResponse response = client.execute(FutureTradingDateRequest.newRequest("CN1901", System.currentTimeMillis()));
System.out.println(response.getFutureTradingDateItem());

```

Response Sample：
```json
{
    "code": 0,
    "timestamp": 1545049282852,
    "message": "success",
    "data": {
        "tradingTimes": [
            {
                "start": 1544778000000,
                "end": 1544820300000
            },
            {
                "start": 1545008400000,
                "end": 1545035400000
            }
        ],
        "timeSection": "Singapore",
        "biddingTimes": [
            {
                "start": 1544777400000,
                "end": 1544778000000
            },
            {
                "start": 1545007500000,
                "end": 1545008400000
            },
            {
                "start": 1545035400000,
                "end": 1545035700000
            }
        ]
    }
}
```
