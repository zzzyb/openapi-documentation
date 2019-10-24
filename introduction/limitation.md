## User Level
Users with greater than 2000 dollars are second level users，less than 2000 dollars and larger than 200 dollars are third level users. Level 1 users need to contact the tiger to get it.
Users with a net asset of less than $200 cannot subscribe and request market data.

### User level limits

Aggrement limit|Level 3 User|Level 2 User|Level 1 User
---|---|---|---
Quote subscription quota	|20|50|100
Quote unsubscribe limit| After 1min | After 1min |No limits
Low frequency API limit	|20|40|	60
High frequency API limit	|60|100	|120
Quote push frequency	|500ms(times)|200ms(times)|50ms(times)
API request time range|Recent 2 years|Recent 5 years|No Limits

### Low Frequency Data API

Socket Name|Request limit per minute
---|---
Market State(market_state)|	20
Stock symbol(all_symbols)	|20
Stock symbol name(all_symbol_names)|	20
Contract list(quote_contract)	|20
Kline(kline)	|20
Shortable stocks list(quote_shortable_stocks)|	20
Quote stock trading information(quote_stock_trade)|	20
Option expiration date(option_expiration)|	20
Option Kline(option_kline)|	20
Future Exchange(future_exchange)|	20
Future Contracts(future_contract_by_contract_code)|	20
Future Contracts(future_contract_by_exchange_code)|	20
Future Contracts(future_continuous_contracts)|	20
Future Current Contract(future_current_contract)|	20
Future trading date(future_trading_date)|	20
Future Kline(future_kline)|	20


### High Frequency Data API

Interface Name|Request limit per minute
---|---
Get Order Number(order_no)|120
Place Order(place_order)|120
Modify Order(modify_order)|120
Cancel Order(cancel_order)|120
Query Order Information(orders)|120
Query active Orders(active_orders)|120
Query inactive Orders(inactive_orders)|120
Query filled Orders (filled_orders)|120
Quote Timeline(timeline)|	120
Quote real time(quote_real_time)|	120
Transaction Details (trade_tick)|	120
Option Quote Summary(option_brief)|	120
Options Transaction Detail (option_trade_tick)|	120
Futures Transaction Detail(future_tick)|	120
Futures real time quote(future_real_time_quote)|	120
