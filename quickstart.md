We provide service only to users with a funded account, you can have access to all the api as soon as you register as a developer.

#### Information required for Registration：

Developer registration address：Please contract us for the detailed registration process.

Field  | Description | If required 
--- | --- | ---
Public Key|RSA public key, used to verify the identity of the developer, public key generation can refer to:[RSA key generation](#rsa)|Yes
IPwhitelist|Only the IP in the whitelist can access the API when provided, you can provide multiple IPs by using ";" to separate them.|No
Callback URL|The user application's callback address, which can receive order status change messages as well as change messages for positions and assets. Callback information can also be received through the subscription interface provided by the SDK.|No


#### Upon registration, you'll get the following info:

* **tigerId**： Used to uniquely identify a developer that is required when requesting an API.
* **account**： The trading account id. It is necessary when requesting informations related to account status.

You can access any API services once you have the tigerID, privateKey and account. 
You can also choose to use the python or java SDK provided by us.


### SDK Code acquisition

Java developers can get code through the Maven repository:

```maven
  <dependency>
    <groupId>io.github.tigerbrokers</groupId>
    <artifactId>openapi-java-sdk</artifactId>
    <version>1.0.8</version>
  </dependency>
```

Github address：

* https://github.com/tigerbrokers/openapi-java-sdk
* https://github.com/tigerbrokers/openapi-python-sdk
* https://github.com/tigerfintech/openapi-java-sdk-demo


### Java SDK Call Example

Take the order number interface as an example:

```java
/**
* tigerId：Can be found on the developer information page
* privateKey: RSA private key corresponding to the uploaded public key
**/
TigerHttpClient client = new TigerHttpClient(serverUrl, tigerId, privateKey);
TigerHttpRequest request = new TigerHttpRequest(ApiServiceType.ORDER_NO);

String bizContent = TradeParamBuilder.instance()
    .account("DU575569")  //account can be found on the developer information page
    .buildJson();

request.setBizContent(bizContent);
TigerHttpResponse response = client.execute(request);
```

Response return sample:

```json
{
    "code":0,
    "message": "success",
    "timestamp": 1530173419060,
    "data": {
        "orderId":105432
    }
}
```

### <span id="rsa">RSA Key Generation</span>

The generation of public and private keys is based on openssl, 
which can be installed via brew on Mac OS.

```
brew install openssl
```

linux：

```
sudo apt-get install openssl
```


The installation for windows is difficult. Please refer to the openssl official website or use the online generation tool as reference. For specific tools, please search for the keyword "RSA certificate online generation".

After installing openssl, please refer to the code below to generate the public and private keys.
The private key should be stored on your local drive and is used to sign the content when requesting the server.The public key needs to be uploaded on the open platform to authenticate the user's request.
Note **The number of generated key bits is 1024**

```
//  Generate private key
openssl genrsa -out rsa_private_key.pem 1024 

// Java Developers need to convert the private key to PKCS8 format, which is not required for the Python SDK.
openssl pkcs8 -topk8 -inform PEM -in rsa_private_key.pem -outform PEM -nocrypt 

// Generating a public key
openssl rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem 
```

**Note, Java and the Python SDK use a different private key format. When using the SDK, you should check the correct format of the private key.**

The private key used by Java is in PKCS#8 format. JAVA developers need to handle the private key as follows

* Remove the front and end, line breaks, and spaces from the private key that pkcs8 outputs in the console as the developer private key.
* After the above steps, the developer retains the private key (the private key printed on the console) and submits the public key (stored in the rsa_public_key.pem file) to the open platform for information encryption and decryption.

The private key before processing is as follows

```
-----BEGIN PRIVATE KEY-----
MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBAMSTmfyJXr1ngtNm
mfPqgWFRR2N2s6p/w5gXqf1uh2dY2tGu9c1WF8LMhZ/Ek6M7m2BXwjUtMv/rh5Cm
w7xpizY419FBOWeKZl1Ru7NFQg52lbnMnDKjtnqG5Gl911WtnvHIAodf1v5yUHRL
k+MPIpb+QonZNiExCzeh3pegUUFhAgMBAAECgYEAnNuGyWu4LHzXeObrPCZI/SXF
SEnkzc1LfyaK3459/2p4mU76FtJ2/VsD2Vwbzun2buc4MgSSKIKB11wq3kJ98O1R
ntu7VY53JiBXcXqDDscLXq0VZJ3BIvozVuzHNflmmE328gK9LTA788J9ohbsObXm
Htk06szF7sg0aANJNV0CQQDknmCvfimRDcEd7IiwRCqu2lvNVe8An6yKzhTYUEHP
p2Y2QfJm61CRQNNIgC1+3tpT/4x+0p63roOJiQdkd/5PAkEA3B7J760gpG8A17JG
bn4RKMBRVK66An1w39W6CF47fdcIMjlb6MjnQAd/dRn2bHGw5WrZNZ3HSLQ/mI0Y
PdcJTwJAfdxzfioG2ESqPL8rwV7F4N12DOVyXvWJGCG8eBo3IQsXymcj/GUwRcda
il+GrIIj0Hqv7mIl3xnEcMNvvnARIQJAZGKmNWf/Ov5ko/nppPpZWPxcGwKUUg5j
K7GM5cQT3Y/zbPQ7ti3pSIoi1oTAnTQ8OGRCKvGJsN6DIk82fv1SgQJBAKfKxCyQ
uuNxLLG0GVLMUWGg/pb7LS0aUcOhmqTpxBEQNx7prH+NHBvXRQOQD1kyeAxvzg1U
MEsqa98xrp5ypVA=
-----END PRIVATE KEY-----
```

The private key used by the Python SDK is the PKCS#1 RSA format, as shown in the following

```
-----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQDEk5n8iV69Z4LTZpnz6oFhUUdjdrOqf8OYF6n9bodnWNrRrvXN
VhfCzIWfxJOjO5tgV8I1LTL/64eQpsO8aYs2ONfRQTlnimZdUbuzRUIOdpW5zJwy
o7Z6huRpfddVrZ7xyAKHX9b+clB0S5PjDyKW/kKJ2TYhMQs3od6XoFFBYQIDAQAB
AoGBAJzbhslruCx813jm6zwmSP0lxUhJ5M3NS38mit+Off9qeJlO+hbSdv1bA9lc
G87p9m7nODIEkiiCgddcKt5CffDtUZ7bu1WOdyYgV3F6gw7HC16tFWSdwSL6M1bs
xzX5ZphN9vICvS0wO/PCfaIW7Dm15h7ZNOrMxe7INGgDSTVdAkEA5J5gr34pkQ3B
HeyIsEQqrtpbzVXvAJ+sis4U2FBBz6dmNkHyZutQkUDTSIAtft7aU/+MftKet66D
iYkHZHf+TwJBANweye+tIKRvANeyRm5+ESjAUVSuugJ9cN/VugheO33XCDI5W+jI
50AHf3UZ9mxxsOVq2TWdx0i0P5iNGD3XCU8CQH3cc34qBthEqjy/K8FexeDddgzl
cl71iRghvHgaNyELF8pnI/xlMEXHWopfhqyCI9B6r+5iJd8ZxHDDb75wESECQGRi
pjVn/zr+ZKP56aT6WVj8XBsClFIOYyuxjOXEE92P82z0O7Yt6UiKItaEwJ00PDhk
QirxibDegyJPNn79UoECQQCnysQskLrjcSyxtBlSzFFhoP6W+y0tGlHDoZqk6cQR
EDce6ax/jRwb10UDkA9ZMngMb84NVDBLKmvfMa6ecqVQ
-----END RSA PRIVATE KEY-----
```
