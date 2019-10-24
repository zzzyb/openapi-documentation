---
title: subscription description
keywords: Tiger Brokers, Tiger API, Open API
description: Document for Tiger API and Open API
---

## Introduction of Service

Websocket protocol is used for client and server communication, stomp sub-protocol is used for data transmission, and users can develop the relevant language according to their preference. Such as Java, Python, Javascript and so on.



## Instruction for Long Connection

* Long connection implementation: both websocket protocol and websocket+stomp are supported. Corresponding to different ports. Websocket port :8883, websocket+stomp port: 8887.
* SSL protocol support version: TLSv1, TLSv1.1, TLSv1.2, TLSv1.3.
* Only one connection can exist simultaneously with the same tigerid. If you make multiple connections, only the most recently subscribed connections will be pushed.

Weblink for socket connection:
```java
//online
wss://openapi.itradeup.com:8887/stomp

```

### 1.Websocket Protocol Specification

webscoket support version: TLSv1.3

The format of Frame only supports binary

### 2.Stomp Protocol Specification

OpenApi supports a collection of Stomp, covering Stomp 1.2/1.1/1.0

#### 2.1 CONNECT Frame

After the Channel establishes a handshake connection, the client sends a Connect request to the server.

CONNECT Frame must include the following information:

```java
{
	"command": "CONNECT",
	"headers": {
		"accept-version": "1.2",
		"host": "localhost",
		"login": "1",
		"passcode": "your sign"
	}
}
```
Where the field accept-version accepts a single version number or a list of version Numbers ("," as the separator), if null, Stomp 1.0 is processed; For version list, select the highest version.

For example: "accept-version": "1.0,1.1,1.2", the server will follow Stomp as it thinks the client can support version 1.2.

Passcode is the signature of tigerid signed with the developer's own private key. In the example below, we use 1 as our tiger id.

#### 2.2 CONNECTED Frame

The server receives a legitimate Connect request and sends a Connected Frame to the client, indicating that Stomp successfully established the connection.

CONNECTED Frame will include the following information：

```java
{
	"command": "CONNECTED",
	"headers":{
		"version": "1.2",
		"id":"0",
		"session": "367c730a"
	}
}
```

among them：

Version is the Stomp version number received by the server.

Id defaults to 0; If the client sends CONNECT with the id keyword, the value corresponding to the id is taken

Session is the id of the channel to which the server is linked.	

#### 2.3 SUBSCRIBE Frame

After Channel establishes Stomp connection, the client can subscribe and unsubscribe

The open platform requires clients to issue SUBSCRIBE frames in the following format：

```java
{
	"command": "SUBSCRIBE",
	"headers": {
		"id":"",
		"destination": "",
		"subscription": "",
	}
}
```

If the SUBSCRIBE Frame issued by the client does not contain destination, the server generates a destination based on the subscription.

The subscription requirement corresponds to the destination one to one.

The subscription range is: Asset, Position, OrderStatus, Quote.

##### 2.3.1 Market data Subscription

You need to append symbols and keys keys to the universal SUBSCRIBE Frame，for example：

```java
{
	"command": "SUBSCRIBE",
	"headers": {
		"id":"2",
		"destination": "/quote/symbols",
		"subscription": "Quote",
		"symbols":"GOOG,AAPL,02318,00700",
		"keys":""

	}
}
```

##### 2.3.2 Other Subscription

Please use the universal SUBSCRIBE Frame to subscribe Asset, Position, Orderstatus，for example：

```java
{
	"command": "SUBSCRIBE",
	"headers": {
		"id":"3",
		"destination": "/asset/symbols",
		"subscription": "Asset"
	}
}
```

#### 2.4 UNSUBSCRIBE Frame

Quote subscription can be unsubscribed several times; The Asset, Position, OrderStatus subscription can only be unsubscribed once.

##### 2.4.1 Unsubscribe Market data

It is required that the Frame must contain symbols, otherwise an error will be raised. The format is as follows：

```java
{
	"command": "UNSUBSCRIBE",
	"headers": {
		"id": "2", 
		"subscription": "Quote",
		"symbols":"GOOG, AAPL"
	}
}
```

##### 2.4.2 Unsubscribe other data

as follows：

```java
{
	"command": "UNSUBSCRIBE",
	"headers": {
		"id": "3", 
		"subscription": "Asset"
	}
}
```

#### 2.5 MESSAGE Frame

Once the subscription is successful, the data is sent to the client in the form of a MESSAGE Frame, as follows：

```java
{
	"command": "MESSAGE",
	"headers": {
		"id": "1",
		"session": "367c730a",
		"ret-type": "112",
		"destination","",
		"subscription": "1",
		"message-id":29,
		"content-length": 30,
		"content-type": "text/plain"
	},
	"content": {
	"code":,
	"message":"success"
	}
}
```

The fields "destination" or "subscription" in the message frame could be empty for compatibility reasons

#### 2.6 Error Frame

If an error is triggered, the server will send an Error Frame to the client and the channel will be closed, as shown below：

```java
{
	"command": "ERROR",
	"headers": {
		"id": "0",
		"session": "channel-id",
		"receipt-id": "",
		"content-length": 28,
		"content-type": "text/plain"
	},
	"content": {
		"code": "40028",//Error Code
		"message": "subscribe error"
	}
}
```

Note that id or receipt-id can be empty.

#### 2.7 DISCONNECT Frame

The client can disconnect from server by sending Disconnet Frame. The format is as follows:

```java
{
	"command":"DISCONNECT",
	"headers":{
		"id": 4
	}
}
```



## SDK instructions

### 1. Implement Callback Interface
```java
/**
* Implement a WebSocketCallback for handling subscription data
**/
ApiComposeCallback callback = new DefaultApiComposeCallback();


/**
 * After successful connection, one of the following two callback methods will be called back. The user needs to implement the following two callback methods
 */
void connectAck();

/**
 * @param serverSendInterval Server guarantees the minimum interval of sending heartbeat, 0 means server does not send heartbeat
 * @param serverReceiveInterval The interval between when the server wants to receive the client's heartbeat, 0 indicates that the server does not want to receive the client's heartbeat
 */
void connectAck(int serverSendInterval, int serverReceiveInterval);

```

### 2.Construct certification class
```java
/**
* input parameters:
* tigerId: API platform authorized ID
* privateKey: Pass in your private key, and the build method will sign the content of tigerId, such as tigerId=2015xxxx, and then use the private key to sign 2015xxxx
**/
ApiAuthentication authentication = ApiAuthentication.build("tigerId","privateKey");
```

### 3.Generate Client
```java
/**
* input parameter:
* socketUrl:socket link server address
* authentation:authorization class generated above
* callback:callback class generated above
**/
WebSocketClient client = new WebSocketClient("wss://openapi.itradeup.com:8883", authentication, callback);
```

### 4.Connect Interface
```java
client.connect();
```

### 5.Call Service
```java
client.placeOrder(stockOrder);
```

### 6.End Service
```java
/**
* If there is no subsequent action to stop the service, no asynchronous message push will be received thereafter
* Initializing disconnect() will cancel the subscribed quotes at the same time
*/
client.disconnect();
```


## Subscription Interface List

Interface | Details 
---|---
void subscribe(Subject subject) | Subscription Topic Interface 
void subscribe(Subject subject,List&lt;String&gt; focusKeys) | Subscribe to the topic interface, which allows you to specify the key to focus on 
void cancelSubscribe(Subject subject) | Unsubscribe from the topic interface 
void subscribeQuote(Set&lt;String&gt; symbols)| Subscribe quotes data interface 
void cancelSubscribeQuote(Set&lt;String&gt; symbols)| Unsubscribe from the quote data interface 
void getSubscribedSymbols()|Query the subscribed symbol list interface
subscribeOption(Set&lt;String&gt; symbols)|Subscription option interface
cancelSubscribeOption(Set&lt;String&gt; symbols)|Unsubscribe option interface
subscribeFuture(Set&lt;String&gt; symbols)|Subscription futures interface
cancelSubscribeFuture(Set&lt;String&gt; symbols)|Unsubscribe futures interface


