### Contract Type

identification|Contract type
---|---
STK|Stock
OPT|US stock options
WAR|Hong Kong stock market warrant 
IOPT|Hong Kong stocks CBBC
CASH|Foreign currency  exchange
FUT|Futures
FOP|Futures option

### Currency Type

identification|Currency Type
---|---
USD|US dollar 
HKD|Hong Kong dollar
CNH|Renminbi

### Order Status

Status | Status code |Description
--- | --- | ---
Invalid|-2|Invalid state
Initial|-1|Order initial state
PendingCancel|3 | Pending cancel 
Cancelled |4| Order cancelled 
Submitted|5|Order submitted
Filled |6| Order completed 
Inactive |7| Order inactive 
PendingSubmit |8| Pending submit 

### Account Status

Status|Description
--- | ---
New| New account 
Funded| Funded 
Open|Open
Pending|Pending
Abandoned|Abandoned
Rejected|Rejected
Closed|Closed
Unknown|Unknown

### Order Type

Type|Description
--- | ---
MKT|Market Order
LMT|Limit Order
STP|Stop-loss order
STP_LMT|Stop limit order
TRAIL|Trail stop limit order

### Account Type

Type|Description
--- | ---
CASH | Cash account 
MGRN | Reg T margin account 
PMGRN | Portfolio margin 

### <span id="subject">Subscription Topic</span>

Subject|Description
---|---
OrderStatus|Order status change
Asset|Asset
Position|Position

### <span id="ktype">Kline Type</span>

 Type  | Description 
---|---
day|DayK
week|WeeklyK
month|MonthlyK
year|YearlyK
min1|1mins
min5|5mins
min15|15mins
min30|30mins
min60|60mins

### <span id="orderChange">Order Change  </span>

* OrderChangeKey

Field|Description
---|---
id|Global unique order number
account|User account
type|Type
timestamp| Server time                              
orderId|Account self-increament order number
name|Stock name
latestPrice|Latest price
symbol|Stock symbol
action| Buy/sell                                 
orderType| Order Type                               
secType|Security Type
currency|Currency Type
localSymbol|Stock code (Hong Kong stocks are used to identify warrants and CBBCs)
originSymbol|Original stock symbol
strike|Bottom price option, warrant, CBBC exclusive
expiry|Expiration date Option, warrant, CBBC exclusive
right|Option direction PUT/CALL Option, Warrant, CBBC Exclusive
limitPrice|Limit Price
auxPrice|Trail Price
trailingPercent|Prailing percentage
timeInForce|Effective time
goodTillDate|GTD time，Format 20060505 08:00:00 EST
outsideRth|True allows pre- and post-trade (pre-emptive US stocks)
totalQuantity|Total quantity
filledQuantity|Filled quantity
avgFillPrice|Average fill price
lastFillPrice|last filled price
openTime|Order creation time
latestTime|Latest order change time
remaining|Unexecuted shares
status|Order status
source|order source
liquidation|liquadtion value
errorCode|Error code
errorMsg|Error message
errorMsgCn|Error message description
errorMsgTw|Error description

### <span id="positionChange">Position Change</span>

* PositionChangeKey

Field|Description
---|---
account|Account user
symbol|Stock symbol
name|Stock name
type|Type
timestamp|Server time
localSymbol|Stock code (Hong Kong stocks are used to identify warrants and CBBCs)
originSymbol|Original stock symbol
secType|Contract type
market|Trade market
currency|Currency type
latestPrice|Latest price
marketValue|Market value
position|Position
averageCost|Average cost
unrealizedPnl|Floating profit/loss
expiry|Expiration date Option, warrant, CBBC exclusive
strike|Underlying price option, warrant, CBBC exclusive
right|Option side PUT/CALL Option, Warrant, CBBC Exclusive
multiplier|1 lot unit, option, warrant, bull and bear certificate exclusive


### <span id="assetChange">Asset Change</span>

* AssetChangeKey

Field|Description
---|---
account|User account
type|Type
timestamp|Server Time
currency |Currency type
deposit|Depoist
buyingPower |Buying power
cashBalance |Cash balance
grossPositionValue |Gross Position Value
netLiquidation |Net Liquidation Value
equityWithLoan |Equity loan value (including loan value assets)
initMarginReq |Initial margin requirement
maintMarginReq |Current maintenance margin
availableFunds |Available funds (equityWithLoan - initial margin)
excessLiquidity |Residual liquidity (equityWithLoan - maintenance margin)
dayTradesRemaining |Day trades remaining
dayTradesRemainingT1 |Day trades remainingT+1
dayTradesRemainingT2 |Day trades remaining T+2
dayTradesRemainingT3 |Day trades remaining T+3
dayTradesRemainingT4 |Day trades remaining T+4

### <span id="quoteChange">Market change</span>

* QuoteChangeKey

Field|Description
---|---
symbol|stock symbol
type|Type
timestamp|Server time
latestPrice|Latest price
latestTime|Latest time
preClose|last trading day closing price
volume|Transaction volume
open|Opening price
close|Closing price
high|Highest price
low|Lowest price
marketStatus|Market status
askPrice|Ask price
askSize|Ask volume
bidPrice|Bid price
bidSize|Bid Size
p|Price per minute
a|Average price per minute
t|Minutes
v|Transaction volume per minute

