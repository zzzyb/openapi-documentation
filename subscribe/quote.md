title: quote subscription
keywords: Tiger Brokers、Tiger API、 Open API
description: Document for Tiger API and Open API

### Interface Specification
API provides subscription for transaction information and quote, which could be used to get real-time updates.

- The API for push notification of subscription is asynchronous interface, and users could get the result through implement the interface named ApiComposeCallback.
- The type of feedback from the Callback Inferface is JSONObject, which is similar to the strcture of Map and could get value through a specific key. For more information about the Callback Interface, please read the later part of this document.


### Subscribe market data 

Automatically identify the type of subscription based on symbol:

* **stock symbol：** example：AAPL

* **option symbol：** There are two types of available symbols:

  one is : symbol name + expiration date + strike price + side, divided with blanks. For example: (AAPL 20190329 182.5 PUT).

  The other is identifier, which will get back to the specific field when looking up option quote. For example: (SPY   190508C00290000)

* **futures symbol：**For example：CL1901 or CLmain

```java
void subscribeQuote(Set<String> symbols)
```

Input paremeters：

Parameter | Type | If Required | Details 
---|---|---|---
symbols|Set&lt;String&gt;|Yes|Stock code list

Response result：

```java
/**
* return to real-time quote information
*/
void quoteChange(JSONObject jsonObject)
/**
* return to option quote information
*/    
void optionChange(JSONObject jsonObject)
/**
* return to futures quote information
*/
void futureChange(JSONObject jsonObject)
    
/**
* subscribe successfully or not
*/
void subscribeEnd(JSONObject jsonObject)
```

### Push Notification data

Field|Explanation
---|---
symbol|Stock code
type|Type
timestamp|Server time
latestPrice|Latest Price
latestTime|Latest time
preClose|last trading day closing price
volume|Transaction volume
open|Open price
close|Closing price
high|Highest price
low|Lowest price
marketStatus|Market status
askPrice|Ask price
askSize|Ask size
bidPrice|Bid price
hourTradingTag|Symbol before/after market，range：before stock market open or after stock market close
hourTradingLatestPrice|latest price before stock market open/after stock market close
hourTradingLatestTime|Transaction time under the latest price before stock market open/ after stock market close
hourTradingVolume|Transaction volume before the stock market open
bidSize|Bid size
mi|Abbreviation for minute, which means the time-trend of the most recent minute, including: (p-per minute price, a-average per-minute price, t- time, v-transaction volume per minute)

### cancel subscription

```java
/**
* cancel subscription
* symbol type is the same as the subscription time
*/
String cancelSubscribeQuote(Set&lt;string&gt; symbols)

```

response result

```java
/**
* callback interface：
*/
void cancelSubscribeEnd(JSONObject jsonObject)
```

Input parameters

Parameter | Type | If Requird | Details 
---|---|---|---
symbols|Set&lt;string&gt;|Yes|stock code list

### Interface List for querying the subscribed symbol
```java
void getSubscribedSymbols()
```

 return result：

```java
/**
* callback interface：
*/
void getSubscribedQuoteEnd(SubscribedSymbol subscribedSymbol)
```

### subscription example
```java
  //fill in tigerId and privateKey when subcription, returning ApiComposeCallback, example is DefaultApiComposeCallback
  private static WebSocketClient client =
      new WebSocketClient("wss://openapi.itradeup.com:8887/stomp", ApiAuthentication.build("tigerId", "privateKey"),
          new DefaultApiComposeCallback());

  public static void subscribe() {

    client.connect();

    Set<String> symbols = new HashSet<>();

    //stock subscription
    symbols.add("AAPL");
    symbols.add("SPY");

    //futures subscription
    symbols.add("ESmain");
    symbols.add("ES1906");

    //one of option subscription methods
    symbols.add("TSLA 20190614 200.0 CALL");

    //the other option subscription methods
    symbols.add("SPY   190508C00290000");

    //customize the interested field about performance subscription
    List<String> focusKeys = new ArrayList<>();
    focusKeys.add("symbol");
    focusKeys.add("volume");
    focusKeys.add("askPrice");
    focusKeys.add("bidPrice");

    //subscribe the interested field
    //client.subscribeQuote(symbols, focusKeys);

    //subscribe related symbol
    client.subscribeQuote(symbols);

    //review details about subscription
    client.getSubscribedSymbols();

    //suggest disconnect after transaction time frame, disconnection would erase the subscription records automatically
    //client.disconnect();
  }
```

### Callback Interface
* ApiComposeCallback
  
Field|Explanation
---|---
void getSubscribedSymbolEnd(SubscribedSymbol subscribedSymbol)|Get subscription symbol
void quoteChange(JSONObject jsonObject)|Notification for change on subscription
void optionChange(JSONObject jsonObject)|Notification for change on option subscription
void futureChange(JSONObject jsonObject)|Notification for change on futures subscription
void cancelSubscribeEnd(JSONObject jsonObject)|Cancel callback interface for subscription
void error(String errorMsg)  | Callback for exception 
void error(int id, int errorCode, String errorMsg)|Callback for exception
void connectionClosed()|close connection
void connectAck()|connection successful

