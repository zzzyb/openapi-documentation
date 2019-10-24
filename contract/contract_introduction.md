### Contract

A contract represents a trading asset defined by the exchange. We use contract objects to identifiy the asset when placing orders or requesting market data.

Three commonly used contracts are stock contracts，options contracts and futures contracts。

Most Contracts is defined by the following attributes:

* Security symbol (symbol)
* Contract Type(security type)
* Currency Type (currency)
* Stock Exchange market (exchange)

The majority of stocks, CFDs, index or foreign exchange can be uniquely identified by the four attributes mentioned above.

However, more complex contracts(such as options and futures) require some additional information.

Below are the most common types of contracts。

#### Stock

```
        Contract contract = new Contract();
        contract.symbol ="TIGR";
        contract.secType ="STK";
        contract.currency ="USD"; //Not required，order default USD
	contract.market = "US"; //Not required，order default US
```


#### Foreign Exchange

```
        Contract contract = new Contract();
        contract.symbol ="EUR";
        contract.secType ="CASH";
        contract.currency ="GBP";
        contract.exchange ="IDEALPRO"; //Not required
```



#### Option

```
        Contract contract = new Contract();
        contract.symbol ="AAPL";
        contract.secType ="OPT";
        contract.currency ="USD";
        contract.expiry="20180821";
        contract.strike = 30;
        contract.right ="CALL";
        contract.multiplier =100.0;
	contract.market = "US"; //Not required
```

#### Futures

```
        Contract contract = new Contract();
        contract.symbol ="CL1901";
        contract.secType ="FUT";
        contract.exchange ="SGX";
        contract.currency ="USD";
        contract.expiry="20190328";
        contract.multiplier=1.0;
```
