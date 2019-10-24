---
title: Framework
keywords: Tiger Securities、Tiger API、 Open API
description: Tiger Open API help document
---

## Framework

The Tiger Open Platform provides most of the services over the HTTP interface, and most of the API functionality is accessed through a simple HTTP POST request. At the same time, it also provides a socket interface to push the latest market information (implemented by WebSocket/Stomp protocol).

## Request Address

Environment | Request Address |Content-Type| Request method 
--- | --- | --- | ---
Live environment | https://openapi.itradeup.com/gateway | application/json | POST

## Public Request Parameter

Parameter | Type | If required | Maximum length | Description | Sample value 
--- | --- | --- | --- | --- | ---
tiger_id    | string | Yes | 10   | ID assigned to the developer | 20150129
method      | string | Yes | 128  | API name, see:[method instructions](#method) | place_order
charset     | string | Yes | 10   | Please use coding format, currently supports ：UTF-8 | UTF-8
sign_type   | string | Yes | 10   | The type of signature algorithm used by the merchant to generate the signature string. Currently supports RSA. | RSA
timestamp   | string | Yes | 19   | Request time, format "yyyy-mm-dd HH:mm:ss"	| 2018-05-09 11:30:00
version     | string | Yes | 3    | API version, current version supports：1.0, 2.0.Please check the API documentation for specifics | 1.0
biz_content | string | Yes | -    | The set of request parameters, except for the public parameters, all the  parameters are included in this parameter, specifically refer to the documents of each API interface. |
sign        | string | Yes | 344  | Request signature string, see for details:[Signature rule](#sign)	| See example 
account_type| string | No | -    | Account type, must be passed when token authentication, type includes: [GLOBAL, STANDARD] |
access_token| string | No | -    | Token authentication must be included. It is used to authenticate the user. It can be obtained through the login API. |
trade_token | string | No | -    | Optional for token authentication, must included in place order API, can be obtained through the transaction password API |


## Public Response Parameter

Parameter | Type | If required | Description | Sample value 
--- | --- | --- | --- | ---
code            | string | Yes | Return code,: [code field description](#code) | 40001
message         | string | Yes | Return code description, see: code field description | post param is empty
data            | string | No | Response content, see the specific API interface documentation for details. | -
timestamp       | string | Yes | Timestamp returned by API | 1525849042762



## Request Sample
```json
{
  "tiger_id":"1",
  "charset":"UTF-8",
  "sign_type":"RSA",
  "version":"1.0",
  "timestamp":"2018-09-01 10:00:00",
  "method":"order_no",
  "biz_content":"{\"account\":\"DU575569\"}",
  "sign":"QwM4MCdffJ5WK59f+dbFvKMn5Qqw2A5GTA8g0XIAp/Fsvb5fbZUwYzxjznx0jO7VO9Npbzd+ywR6VrMz4liblTMPGDvDnPJP0rGUVF+xbj/3MBr3vFZ25XheyjfHIpP6f+qhNkn9KdFsviohZAWeplkYjV+OyxwMQmpnkP/vll4="
}
```

## Response Sample
```json
{
    "code": 0,
    "message": "",
    "timestamp": 1525849042762,
    "data": {
        "orderId":10000164
    }
}
```

## <span id="method">Method Instructions</span>

Different methods correspond to different requests, corresponding to the method field in the public request parameter, for example, method=place_order is an order request.
If you use the SDK provided by the open platform, you don't need to care about this field. If you want to encapsulate the request yourself, you can refer to the following method definition.

method|Description
--- | ---
order_no             |Get Order Number before place order
place_order          |Plance Order
cancel_order         |Cancel Order
modify_order         |Modify Order
market_state         |Market Sate   
all_symbols          |Get Stock Symbol
all_symbol_names     |Get Stock Symbol and Name
brief                |Get stock brief
stock_detail         |Get stock details
timeline             |Get timeline data
hour_trading_timeline|Get pre-market anf after-hours data
kline                |Kline data
trade_tick           |Trade tick data
contract             |Contract data
assets               |Get assets
positions            |Get positions
orders               |Get order list
filled_orders        |filled order list
active_orders        |Active order list
inactive_orders      |Inactive order list
quote_contract       |Quote contract list
quote_real_time      |Real time quote
quote_shortable_stocks|Shortable stocks quote
quote_stock_trade    |Stock trade quote
financial_daily      |Daily financial data
financial_report     |Financial report data
option_expiration    |Option expiration date
option_chain         |Option chain
option_brief         |Option quote
option_kline         |Option kline
option_trade_tick    |Option trade tick
future_exchange      |Future exchange list
future_contract_by_contract_code|Futures contract details
future_contract_by_exchange_code|Tradeable contract under the exchange
future_continuous_contracts|Future continuous contracts
future_current_contract|Future current contract
future_kline|Future Kline
future_real_time_quote|Future real time quote
future_tick|Future tick data
future_trading_date|Future trading date

## <span id="code">Code Insturctions</span>

Type of error returned corresponding to the code in the public response parameter

code|Description
--- | ---
0     | Request successful 
1     | Server Error 
2     | Network timeout 
3     | Client Server Error 
4     | Access denied 
5     | Socket Limit 
1000 | General parameter problem 
1100 | Logical parameter problem 
1200 | Global account order problem 
1300 | Standard account ordering problem 
1400 | Demo account ordering problem 
2000 | Quotes - Basic data problem 
2100 | Quotes - Stock Data Issues 
2200 | Quotes - Option data problem 
2300 | Quotes - Futures Data Issues 
2400 | Quotes - User Data Issues 
3000 | Subscribtion data issues 


### Signature Instructions

Each request will generate a corresponding signature. After receiving the request, the open platform will use the RSA public key uploaded by the developer to perform signature verification.
The purpose of the signature is to ensure the integrity of the information transmission and the authenticity of the identity of the trader.
When the platform returns the data, the signature will be added as well, and the returned data will also be signed and verified in the implementation of the SDK.
If it is a self-implemented request encapsulation, you must also verify the authenticity of the returned data.

The public key used to verify the public platform is as follows:

```
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDNF3G8SoEcCZh2rshUbayDgLLrj6rKgzNMxDL2HSnKcB0+GPOsndqSv+a4IBu9+I3fyBp5hkyMMG2+AXugd9pMpy6VxJxlNjhX1MYbNTZJUT4nudki4uh+LMOkIBHOceGNXjgB+cXqmlUnjlqha/HgboeHSnSgpM3dKSJQlIOsDwIDAQAB
```

### How to sign the content

* The parameters (except sign) are arranged in lexicographic order to form a string. The format of the string refers to the following example.
* The generated string is then signed by the access party using its own private key.
* The generated signature is assigned to sign and passed along with other parameters to the api.

The api service verifies the parameters and signatures (where the signature algorithm is described below).

Request content to be signed (json format):

```json
{"tiger_id": "20151237", "charset": "UTF-8", "sign_type": "RSA", "version": "1.0", "method": "market_state", "biz_content": "{\"market\": \"US\"}", "timestamp": "2018-11-26 15:31:19"}
```

According to the key, the dictionary is sorted to form a signed string as follows:

```json
biz_content={"market": "US"}&charset=UTF-8&method=market_state&sign_type=RSA&tiger_id=20151237&timestamp=2018-11-26 15:31:19&version=1.0
```

The content actually used for the post request (json format, including the signature)

```json
{"tiger_id": "20151237", "charset": "UTF-8", "sign_type": "RSA", "version": "1.0", "method": "market_state", "biz_content": "{\"market\": \"US\"}", "timestamp": "2018-11-26 15:33:27", "sign": "tP1NandnEdB7LTiYoMGUF4="}
```

### Signature Algorithm

The signature algorithm code is already encapsulated in the SDK. If you call the SDK directly, you don't need to implement the signature algorithm yourself.

### Java Version

```java
import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.security.KeyFactory;
import java.security.PrivateKey;
import java.security.Signature;
import java.security.spec.PKCS8EncodedKeySpec;
import java.util.Base64;

public class SignatureUtil {

  /**
   * 
   *
   * @param content content to be signed
   * @param privateKey 
   * @param charset : UTF-8
   * @return content after signed.
   */
  public static String rsaSign(String content, String privateKey, String charset) {
    try {
      PrivateKey priKey = getPrivateKeyFromPKCS8("RSA",
          new ByteArrayInputStream(privateKey.getBytes()));

      Signature signature = Signature.getInstance("SHA1WithRSA");

      signature.initSign(priKey);

      if (StringUtil.isEmpty(charset)) {
        signature.update(content.getBytes());
      } else {
        signature.update(content.getBytes(charset));
      }

      byte[] signed = signature.sign();

      return new String(Base64.getEncoder().encode(signed));
    } catch (Exception e) {
      throw new RuntimeException("rsa sign error:", e);
    }
  }

  public static PrivateKey getPrivateKeyFromPKCS8(String algorithm, InputStream ins)
      throws Exception {
    if (ins == null || StringUtil.isEmpty(algorithm)) {
      return null;
    }

    KeyFactory keyFactory = KeyFactory.getInstance(algorithm);
    byte[] encodedKey = StreamUtil.readText(ins).getBytes();
    encodedKey = Base64.getDecoder().decode(encodedKey);

    return keyFactory.generatePrivate(new PKCS8EncodedKeySpec(encodedKey));
  }
}
```

### Python Version

```python
import base64
import json
import platform

from Crypto.Signature import PKCS1_v1_5
from Crypto.Hash import SHA
from Crypto.PublicKey import RSA

python_version = platform.python_version()

if python_version.startswith("3"):
    PYTHON_VERSION_3 = True
else:
    PYTHON_VERSION_3 = False

'''
get sign content
'''
def get_sign_content(all_params):
    sign_content = ""
    for (k, v) in sorted(all_params.items()):
        value = v
        if not isinstance(value, str):
            value = json.dumps(value, ensure_ascii=False)
        sign_content += ("&" + k + "=" + value)
    sign_content = sign_content[1:]
    return sign_content
        
'''
sign with rsa
'''
def sign_with_rsa(private_key, sign_content, charset):
    if PYTHON_VERSION_3:
        sign_content = sign_content.encode(charset)
    private_key = fill_private_key_marker(private_key)
    signature = PKCS1_v1_5.new(RSA.importKey(private_key)).sign(SHA.new(sign_content))
    sign = base64.b64encode(signature)
    if PYTHON_VERSION_3:
        sign = str(sign, encoding=charset)
    return sign

'''
verify the sign
'''
def verify_with_rsa(public_key, message, sign):
    public_key = fill_public_key_marker(public_key)
    sign = base64.b64decode(sign)
    return PKCS1_v1_5.new(RSA.importKey(public_key)).verify(message.encode('utf8'), sign)

def fill_private_key_marker(private_key):
    return add_start_end(private_key, "-----BEGIN RSA PRIVATE KEY-----\n", "\n-----END RSA PRIVATE KEY-----")


def fill_public_key_marker(public_key):
    return add_start_end(public_key, "-----BEGIN PUBLIC KEY-----\n", "\n-----END PUBLIC KEY-----")

def add_start_end(key, start_marker, end_marker):
    if key.find(start_marker) < 0:
        key = start_marker + key
    if key.find(end_marker) < 0:
        key = key + end_marker
    return key
```

### C# Version

```C#
using System;
using Org.BouncyCastle.Crypto;
using Org.BouncyCastle.Crypto.Parameters;
using Org.BouncyCastle.Security;
using System.Text;

public abstract partial class RSAUtil
{
    //C# private key must be pkcs8 format, depend on lib Org.BouncyCastle version is 1.8.1
    public static string sign(string data, string privateKeyPkcs8)
    {
        RsaKeyParameters privateKeyParam = (RsaKeyParameters)PrivateKeyFactory.CreateKey(Convert.FromBase64String(privateKeyPkcs8));
        ISigner signer = SignerUtilities.GetSigner("SHA1WITHRSA");
        signer.Init(true, privateKeyParam);
        var dataByte = Encoding.GetEncoding("UTF-8").GetBytes(data);
        signer.BlockUpdate(dataByte, 0, dataByte.Length);
        return Convert.ToBase64String(signer.GenerateSignature());
    }

    public static bool verify(string data, string publicKeyPkcs8, string signature)
    {
        RsaKeyParameters publicKeyParam = (RsaKeyParameters)PublicKeyFactory.CreateKey(Convert.FromBase64String(publicKeyPkcs8));
        ISigner signer = SignerUtilities.GetSigner("SHA1WITHRSA");
        signer.Init(false, publicKeyParam);
        byte[] dataByte = Encoding.GetEncoding("UTF-8").GetBytes(data);
        signer.BlockUpdate(dataByte, 0, dataByte.Length);
        byte[] signatureByte = Convert.FromBase64String(signature);
        return signer.VerifySignature(signatureByte);
    }
}
```

### C++ Version

```c++
#pragma once

#include <openssl/pem.h>
#include <openssl/rsa.h>
#include <openssl/bio.h>
#include <openssl/err.h>
#include <iostream>


//need encrypt to Base64 format firstly
int Sha1RSASign::encrypt(unsigned char * context, int contextLength, unsigned char * key, unsigned char *encrypted, unsigned int* encryptedLength)
{
	RSA * rsa = createRSA(key, true);
	int ret = -1;

	if (rsa != nullptr)
	{
		unsigned char hash[SHA_DIGEST_LENGTH] = "";
		SHA1(context, contextLength, hash);
		ret = RSA_sign(NID_sha1, hash, SHA_DIGEST_LENGTH, encrypted, encryptedLength, rsa);
		if (1 != ret)
		{
			printf_s("sha1 rsa encrypt sign error,result is [%d], encrypted length is [%d]\n", ret, *encryptedLength);
		}
		RSA_free(rsa);
	}
	return ret;
}

RSA * Sha1RSASign::createRSA(unsigned char * key)
{
	RSA *rsa = nullptr;
	BIO *keybio = BIO_new_mem_buf(key, -1);
	if (keybio != nullptr) {
		rsa = PEM_read_bio_RSAPrivateKey(keybio, &rsa, 0, 0);
		BIO_free_all(keybio);
	}
	else {
		printLastError("Failed to create key BIO");
	}

	if (rsa == nullptr) {
		printLastError("Failed to create RSA");
	}

	return rsa;
}

void Sha1RSASign::printLastError(const char *msg)
{
	char * err = new char[130];
	ERR_load_crypto_strings();
	ERR_error_string(ERR_get_error(), err);
	printf("[%s] ERROR:[%s]\n", msg, err);
	delete []err;
}
```

## Callback interface

Callback interface has two type of implement, one is websocket subscription, another is http callback, the callback url of http can be find on the developer page.

Callback interface can subscribe the change of order, asset and position, also the change of market quote.

Here is the callback response sample:

```json
{
    "subject":"Asset",
    "data":{
        "type":"asset",
        "usdCash":854879.28,
        "account":"DU575567",
        "timestamp":1528719139003
    }
}
```

- Callback type can be: OrderStatus(Order status changed)，Asset（Asset changed），Position（Position changed），Quote（Quote of market changed）
- deails about the field of subscription, please refer to the document.

------



