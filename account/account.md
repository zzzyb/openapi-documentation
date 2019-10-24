### Get Managed Account List

request parameters：
    
Parameter | Type | If required | Description 
--- | --- | --- | ---
account       |string  |  Yes  |Authorized Account(Basic Account/FA):DU575569

Response Result：

Name | Example | Description 
--- | --- | ---
account|DU575569|Trading Account
capability|MRGN|Account Type(CASH:cash account, MGRN: Reg T Margin Account, PMGRN: Portfolio margin account )
status|Funded|Status(New, Funded, Open, Pending, Abandoned, Rejected, Closed, Unknown)

Request Sample:
```json
{
  "tiger_id":"1",
  "charset":"UTF8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"20180501 10:00:00",
  "method":"accounts",
  "biz_content":"{\"account\":\"DU575569\"}",
  "sign":"BCjPrQ2jhEj2Bf+ykN3rBrToLBz+tolcuzZOfzzGsTis9uiKIAVkCuo0pINmxvKS1xlDIEEg9YSEvBLOzYyX96Ez7z4J5WjDC4sdUG8iGRHmiAZcq3a2Z6EEzsFAVSylRqEY/H3yIU10bA51Y3QoildilQM6WUI2LTRghYOzDcQ="
}
```

SDK Request Sample:
```java
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.ACCOUNTS);

String bizContent = AccountParamBuilder.instance()
        .account("DU575569")
        .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response Sample:
```json
{
    "code": 0,
    "message": "success",
    "data":{
        "items":[{
            "account": "DU575569",
            "capability": "MRGN",
            "status": "Funded"
        }]
    }
}
```
