### Interface Specification
The trading API provides a trading-related interface and a subscription interface to obtain real-time information about the changes of accounts, assets and positions.

* The API for push notification of subscription is asynchronous interface, and we could get the result through implement the interface named ApiComposeCallback.
* The type of feedback from the Callback Inferface is JSON Object, which is similar to the strcture of Map and could get value through a specific key. For more information about the Callback Interface, please read the later part of this document.


### Subscription Topic Interface

* **Subject is the content you want to subscribe**，which mainly includes three types, OrderStatus(order), Asset(Asset) and Position(Position).
* **OrderStatus** refers to the push when the OrderStatus changes. The change of the order mainly includes: Submitted(Submitted to the exchange), Cancelled(Cancelled), Inactive(rejected), Filled(Filled), and other intermediate status will not be pushed.
* **Asset push**，**Position push** is the push when the account assets and positions change.


```java
void subscribe(Subject subject)
```

The response is obtained by implementing the callback method:

```java
/** 
* Callback Interface 
* Invoke different interfaces based on different subjects
*/
void assetChange(JSONObject jsonObject)  //Corresponding subject = asset
void positionChange(JSONObject jsonObject) //Corresponding subject = position
void orderStatusChange(JSONObject jsonObject) //Corresponding subject = orderstatus
```

See the following instructions for the changed fields:

### <span id="orderChange">Push for orders</span>

Field|Explanation
---|---
id|Globally unique order number
account|Trading account
type|Type
timestamp|Server time
orderId|Account self-increment order number
name|Security name
latestPrice|Latest price
symbol|Stock code
action|Buy/sell
orderType|Order type
secType|Contract Type
currency|Currency Type
localSymbol|Stock code (Hong Kong shares used for identification of warrant and bull and bear certificates)
originSymbol|Original stock code
strike|The strike price option, warrant round, bull and bear certificate exclusive
expiry|Expiration date option, warrant, bull and bear certificate exclusive
right|Option side PUT/CALL option, warrant, bull and bear certificate exclusive
limitPrice|Limit price
auxPrice|Trail Price
trailingPercent|Trailing percentage
timeInForce|Valid time
goodTillDate|GTD time, format 20060505 08:00:00 EST
outsideRth|Premarket or after-hours trading (exclusive to us stocks)
totalQuantity|Order quantity
filledQuantity|fIlled quantity
avgFillPrice|Average filled cost
lastFillPrice|Last filled price
openTime|Order creation time
latestTime|The latest revision time of the order
remaining|The number of shares not executed
status|Order status
source|Order source
liquidation|Liquidation value
errorCode|Error code
errorMsg|Error description
errorMsgCn|Error description
errorMsgTw|Error description

### <span id="positionChange">Position Push</span>

Field|Explanation
---|---
account|Trading account
symbol|Stock code
name|Stock name
type|Type
timestamp|Server time
localSymbol|Stock code (Hong Kong shares used for identification of warrant and bull and bear certificates)
originSymbol|Original stock code
secType|Security Type
market|Trading market
currency|Currency Type
latestPrice|latest price
marketValue|Market value
position|Position
averageCost|Average cost
unrealizedPnl|unrealized profit/loss
expiry|Expiration date: option, warrant, bull and bear certificate exclusive
strike|Strike price: option, warrent, bull and bear certificate exclusive
right|Option side: PUT/CALL option, warrant, bull and bear certificate exclusive
multiplier|multiplier for option and future contracts 


### <span id="assetChange">Asset Push</span>

Field|Explanation
---|---
account|Trading Account
type|Type
timestamp|Server Time
currency |Currency type
deposit|Deposit and withdraw amount
buyingPower |Buying power
cashBalance |Cash balance
grossPositionValue |Market value of position
netLiquidation |Net liquidation value
equityWithLoan |Equity with loan value (including loan value assets)
initMarginReq |Current initial margin requirement
maintMarginReq |Current maintenance margin requirement
availableFunds |Available funds (equityWithLoan - initial margin)
excessLiquidity |Residual liquidity (equityWithLoan -maintenance margin)
dayTradesRemaining |Number of trades for the remaining days
dayTradesRemainingT1 |Number of trades for the remaining days T+1
dayTradesRemainingT2 |Number of trades for the remaining days T+2
dayTradesRemainingT3 |Number of trades for the remaining days T+3
dayTradesRemainingT4 |Number of trades for the remaining days T+4


### Subscription

* Subscribe to specify subject and interested key(the changeKey field listed above)

```java
void subscribe(Subject subject,List<String> focusKeys)
```


### Unsubscribe Interface

* Unsubscribe specify subject

```java
void cancelSubscribe(Subject subject)
```

### Example for Subscription
```java
//The actual subscription requires the tigerId and privateKey to be populated and the callback interface implemented
  private static WebSocketClient client =
      new WebSocketClient("wss://openapi.itradeup.com:8887/stomp", ApiAuthentication.build("tigerId", "privateKey"),
          new DefaultApiComposeCallback());

  public static void subscribe() {
    //create connection
    client.connect();

    //subscribe order/asset/position
    client.subscribe(Subject.OrderStatus);    
    client.subscribe(Subject.Asset);
    client.subscribe(Subject.Position);

    //Subscribe to the order field of interest
    List<String> focusKeys = new ArrayList<>();
    focusKeys.add("symbol");
    focusKeys.add("account");
    focusKeys.add("latestPrice");
    focusKeys.add("errorCode");

    //Subscribe to the order changes for the fields of interest
    //client.subscribe(Subject.OrderStatus, focusKeys);

    //It is recommended to close the connection during the non-trading period, which will automatically cancel all previous subscription information
    //client.disconnect();
}
```

### Callback Interface (transaction part)

* ApiComposeCallback

Field|Explanation
---|---
void orderStatusChange(JSONObject jsonObject)|Order status change
void positionChange(JSONObject jsonObject)|Position changes
void assetChange(JSONObject jsonObject)|Asset change
void subscribeEnd(JSONObject jsonObject)|Subscribe interface callbacks
void cancelSubscribeEnd(JSONObject jsonObject)|Cancel subscription interface callbacks
void error(String errorMsg)  | Abnormal callback 
void error(int id, int errorCode, String errorMsg)|Abnormal callback
void connectionClosed()|Connection has closed
void connectAck()|successfully connect

