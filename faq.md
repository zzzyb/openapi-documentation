## Issue Feedback

If you have any questions, feel free to contract us through our discord channel https://discord.gg/nbqvAQr
In order to locate the problem as soon as possible and give you feedback, please provide the following information (browser name chrome or firefox etc.)

* Request method, including url and request parameters
* Returned error message

## Common Error

### Signature Error

Response Result：
```json
{"code":40013,"data":"","message":"invalid signature","timestamp":1527732508206}
```
When this error occurs, it may be that the public key of open api is configured as the user's own public key, or the private key is not configured correctly. Please check the configuration carefully.

### Timestamp Conversion

API  default response is in milliseconds, refer to the following method to transfer cost time：

```python
>>>from datetime import datetime
>>>from pytz import timezone

>>>time_stamp = 1546635600000
>>>tz = timezone('Asia/Chongqing')

>>>datetime.fromtimestamp(time_stamp, tz)

datetime.datetime(2019, 1, 5, 5, 0, tzinfo=<DstTzInfo 'Asia/Chongqing' CST+8:00:00 STD>)

```

---

### How to use MarketStatus

Take get_market_status as example. This API returns a list of MarektStatus.

```python
>>>a_list = [MarketStatus({'market': 'US', 'status': 'Pre-market trading', 'open_time': datetime.datetime(2019, 1, 7, 9, 30, tzinfo=<DstTzInfo 'US/Eastern' EST-1 day, 19:00:00 STD>)})]
>>>us_market_status = a_list[0]
>>>us_market_status.market

'US'

```

---

### Data Related Issues

**Missing data**

If data is missing in sandbox environment, try using the formal environment. If it is still missing, please provide timely feedback within the QQ group.

**push_client pushed the unsubscribed stock**

sandbox environment's ID is shared to public. Therefore, this data may be returned because others have subscribed to the relevant securities.



###  Using of simulated ENV

After registering the developer information, an demo account will be automatically generated. You can enter the Tiger trade APP “transaction” tab, and click the top area to switch between the real and demo account.

The order-placing method of the mock offer is the same as that of the production offer. To place order, the Account parameter is passed into the mock offer Account instead of the production offer Account.


### Sandbox, Simulated, Production Relationship

Both simulation and real trading need to be in the real environment.

Sandbox is an independent development environment for development and debugging under certain special scenarios. A separate private key and Tigerid are required.

It is recommended that the user first use the demo account to debug the program.



### Futures Contract Supported

We currently supports the following futures exchanges

```python
      code      name           																	 zone
0      CME  Chicago Mercantile Exchange   									America/Chicago
1    NYMEX  New York Mercantile Exchange  							 	 America/New_York
2    COMEX  New York Mercantile Exchange 										America/New_York
3      SGX  Singapore Stock Exchange	   										  Singapore
4     HKEX  Hong Kong Stock Exchange  		 									 Asia/Hong_Kong
5     CBOT  Chicago Board of Trade  												America/Chicago
6  CBOEXBT  Chicago Board Option Exchange 									America/Chicago
7   CMEBTC  Chicago Mercantile Exchange Bitcoin Futures 		America/Chicago
8      OSE  Osaka Stock Exchange     					 							  Asia/Tokyo
9     CBOE  Chicago Board Options Exchange 									 America/Chicago
```

The list of supported futures contracts is as follows: Actual support is subject to API


| CME | NYMEX | COMEX | SGX | HKEX |
| --- |  --- |  --- | --- | --- |
| MXP - peso </br>MEUR - Micro euro </br>ES - SP500 index </br>CHF - Swiss Francs </br>NZD - New Zealand dollar </br>MAUD - Micro Australian dollar </br>GBP - Pound </br>HE - lean hog </br>RTY - Russell2000 index </br>LE - Live cattle </br>JPY - Yen </br>GF - Fattening Cattle </br>CAD - Canadian dollar </br>EUR - Euro </br>AUD - Australian dollar </br>NQ - NQ100 index </br> | RB - gasoline </br>QM - crude oil</br> PL - platinum </br>HO - Fuel </br>CL - WTIcrude oil </br>PA - Palladium </br>NG - natural gas </br> | MGC - Micro gold</br> HG -brass</br>GC - gold </br>SI - silver </br> | IN - NIFTY index </br> FEF - Iron ore </br> UC - SG China Yuan </br> TW - MSCI Taiwan Index </br> CN - A50 Index </br> TF - natural rubber </br> NK - SGXNikkei</br> | CHH - 120 index</br>PAI - China Ping An Futures</br>MCH - H share index </br>HHI - H share index</br>CNH - HK Chinese Yuan </br>TCH - Tencent Holdings</br>MIU - Xiaomi Corporation </br>HSI - Hang Seng Index </br>MHI - MHImain </br> |


| CBOT | CBOEXBT | CMEBTC | OSE | CBOE |
| --- | --- | --- | --- | --- |
| ZF - 5 years of US debt </br>ZC - corn </br>ZB - 30 years of US debt </br>ZO - oat </br>ZN - 10 years of US debt</br>ZM - Cardamom </br>ZL - Soybean oil</br>UB - Over 30 years of US debt </br>YM - Dow Jones Index</br>TN - Over 10 years of US debt </br>ZW - oat </br>ZS - Soy </br>ZR - paddy </br>ZT - 2 years of US debt </br> | XBT - CBOE Bitcoin | BTC - CME Bitcoin | JNI - OSENikkei </br>JMI - OSE Small Nikkei </br>JTM - Small TOPIX, Tokyo Stock Price Index </br>JTI - TOPIX, Tokyo Stock Price Index </br> | ES - SP500 index |


### Open Futures Trading

Currently we support the futures trading function of the Tiger Global Account.
If the order fails, you need to go to the IB background to check if you have opened the futures trading authority.
Methods as below:

1. Log in using IB client portal with username and password, link https://www.ibkr.com.cn/portal/
2. Access Account Settings
3. Click the Settings button for Trading Permissions at the bottom of the page.
4. Under the futures section, check the exchanges for which you want to open trading permissions. It is recommended to check Hong Kong/Signapore/United States/United States (Floor Based Exchange)


### Issues about Order Status Queries

**Order status is asynchronously updated**

Example: order objext is viewed separately after the create_order, place_order and get_order links, and the differences are compared. 

**1. Create order **

No global id, only account-related order_id, status is NEW

```python
Order({'account': 'U523', 'id': None, 'order_id': 297, 'parent_id': None, 'order_time': None, 'reason': None, 'trade_time': None, 'action': 'BUY', 'quantity': 100, 'filled': 0, 'avg_fill_price': 0, 'commission': None, 'realized_pnl': None, 'trail_stop_price': None, 'limit_price': 0.1, 'aux_price': None, 'trailing_percent': None, 'percent_offset': None, 'order_type': 'LMT', 'time_in_force': None, 'outside_rth': None, 'contract': ES/None/USD, 'status': 'NEW', 'remaining': 100}) 
```

**2. Place order **

Generate global id, status is still NEW. Only id is added to the order object after the palce order. If you want to get the latest order status, you need to query with get_order

```python
Order({'account': 'U523', 'id': 154758864761483264, 'order_id': 297, 'parent_id': None, 'order_time': None, 'reason': None, 'trade_time': None, 'action': 'BUY', 'quantity': 100, 'filled': 0, 'avg_fill_price': 0, 'commission': None, 'realized_pnl': None, 'trail_stop_price': None, 'limit_price': 0.1, 'aux_price': None, 'trailing_percent': None, 'percent_offset': None, 'order_type': 'LMT', 'time_in_force': None, 'outside_rth': None, 'contract': ES/None/USD, 'status': 'NEW', 'remaining': 100})
```

**3. Get order **

Status changed to REJECTED, the reason for the order rejection is added.

```python
Order({'account': 'U523', 'id': 154758864761483264, 'order_id': 297, 'parent_id': 0, 'order_time': 1550115294556, 'reason': '201:Order rejected - Reason: YOUR ORDER IS NOT ACCEPTED. MINIMUM OF 2000 USD (OR EQUIVALENT IN OTHER CURRENCIES) REQUIRED IN ORDER TO PURCHASE ON MARGIN, SELL SHORT, TRADE CURRENCY OR FUTURE', 'trade_time': 1550115294694, 'action': 'BUY', 'quantity': 100, 'filled': 0, 'avg_fill_price': 0, 'commission': 0, 'realized_pnl': 0, 'trail_stop_price': None, 'limit_price': 0.1, 'aux_price': None, 'trailing_percent': None, 'percent_offset': None, 'order_type': 'LMT', 'time_in_force': 'DAY', 'outside_rth': True, 'contract': ES, 'status': 'REJECTED', 'remaining': 100})
```


### API Request Limitation

- Current limit unit: number of requests per minute
- Current-limit type: IP traffic limiting, account traffic limiting, and interface traffic limiting.
- High-frequency interface: market quote related interface (except for obtaining stock code interface), creating and canceling order interface  

Query stock code, name interface is limited to five requests per hour

| IP(request/min) | Tigerid(request/min) | Socket(request/min) | High frequency socket(request/min) |
| --------------- | ------------------- | ---------------- | -------------------- |
| 600 | 600 | 60 | 120 |


## Additional Help

For questions, please add the official QQ group consultation, group number: 869893807.
