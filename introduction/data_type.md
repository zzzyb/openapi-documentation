### Contract Type

Identification|Contract Type
---|---
STK|Stocks
OPT|US Stock Options
WAR|HK Market Warrant 
IOPT|Hong Kong stocks CBBC
CASH|Foreign Currency Exchange
FUT|Futures
FOP|Future Option

### Currency Type

Identification|Currency Type
---|---
USD|US dollars 
HKD|Hong Kong dollar
CNH|RenMinBi Yuan

### Order Status

Status | Status code |Description
--- | --- | ---
Invalid|-2|Invalid State
Initial|-1|Initial Order Status
PendingCancel|3 | Pending Cancel 
Cancelled |4| Cancelled 
Submitted|5|Submitted
Filled |6| Filled 
Inactive |7| Inactive 
PendingSubmit |8| Pending Submit 

### Account Status

Status|Description
--- | ---
New| New Account 
Funded| Funded 
Open|Opened Account
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

### <span id="subject">Subscription </span>

Subject|Description
---|---
OrderStatus|Order status change
Asset|Asset
Position|Position

### <span id="ktype">Kline Type</span>

Type|Description
---|---
day|Daily K Line
week|Weekly K Line
month|Monthly K Line
year|Yearly K Line
min1|1 min K Line
min5|5 mins K Line
min15|15 mins K Line
min30|30 mins K Line
min60|60 mins K Line

### <span id="orderChange">Order Change </span>

* OrderChangeKey

Field|Description
---|---
id|Global unique order number
account|User account
type|Type
timestamp|Server time
orderId|Account auto-incremental order number
name|Stock symbol
latestPrice|Latest price
symbol|Stock symbol
action|Buy/sell
orderType|Order type
secType|Security type
currency|Currency type
localSymbol|Stock symbol (Hong Kong stocks are used to identify warrants and CBBCs)
originSymbol|Original Stock Symbol
strike|underlying price option, warrant, CBBC exclusive
expiry|Expiration date Option, warrant, CBBC exclusive
right|Option side PUT/CALL Option, Warrant, CBBC Exclusive
limitPrice|Limit Price
auxPrice|Trail Price
trailingPercent|Percentage of trailing stop orders
timeInForce|Effective time
goodTillDate|GTD time，format 20060505 08:00:00 EST
outsideRth|true Allow pre- and post-market trading (US stock exclusive)
totalQuantity|Total quantity
filledQuantity|Filled quantity
avgFillPrice|Average fill price
lastFillPrice|last fill price
openTime|Order creation time
latestTime|Latest order change time
remaining|Unexecuted shares
status|Order status
source|Order original source
liquidation|Liquidation
errorCode|Error code
errorMsg|Error message
errorMsgCn|Error message
errorMsgTw|Error message

### <span id="positionChange">Position Change</span>

* PositionChangeKey

Field|Description
---|---
account|User account
symbol|Stock symbol
name|Stock name
type|Type
timestamp|Server time
localSymbol|Stock symbol (Hong Kong stocks are used to identify warrants and CBBCs)
originSymbol|Original stock symbol
secType|Contract type
market|Stock exchange market
currency|Currency type
latestPrice|Latest price
marketValue|Market value
position|Position
averageCost|Average cost
unrealizedPnl|unrealized profit/loss
expiry|Expiration date Option, warrant, CBBC exclusive
strike|underlying price option, warrant, CBBC exclusive
right|Option side PUT/CALL Option, Warrant, CBBC Exclusive
multiplier|1 lot unit, option, warrant, bull and bear certificate exclusive


### <span id="assetChange">Asset Change</span>

* AssetChangeKey

Field|Description
---|---
account|User account
type|Type
timestamp|Server time
currency |Currency type
deposit|Deposit
buyingPower |Buying power
cashBalance |Account cash balance
grossPositionValue |Gross position value
netLiquidation |Net liquidation value
equityWithLoan |Including loan value equity (including loan value assets)
initMarginReq |Initial Margin Required
maintMarginReq |Maintainance Margin Required
availableFunds |Funds available(equityWithLoany - initial margin)
excessLiquidity |excess liquidity (equityWithLoan - maintenance margin)
dayTradesRemaining |Day trades remaining
dayTradesRemainingT1 |Day trades remaining T+1
dayTradesRemainingT2 |Day trades remaining T+2
dayTradesRemainingT3 |Day trades remaining T+3
dayTradesRemainingT4 |Day trades remaining T+4

### <span id="quoteChange">Market Change</span>

* QuoteChangeKey

Field|Description
---|---
symbol|Stock symbol
type|Type
timestamp|Server time
latestPrice|Latest price
latestTime|Latest time
preClose|last trading day closing price
volume|Trading volume
open|Opening price
close|Closing price
high|Highest price
low|Lowest price
marketStatus|Market status
askPrice|Ask price
askSize|Ask volume
bidPrice|Bid price
hourTradingTag|Pre-market after- hours tag, value：pre-market or after market
hourTradingLatestPrice|Pre-market/after-hours price
hourTradingLatestTime|Pre-market/after-hours latest price transaction time
hourTradingVolume|Pre-market volume
bidSize|Bid Size
p|Price per minute
a|Average price per minute
t|Minutes time
v|Transaction volume per minute
