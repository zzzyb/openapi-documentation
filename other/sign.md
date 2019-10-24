### Signature Description

* When a third party calls the API, it needs to provide a signature in the request, mainly to prevent other parties from making a mistake.
* Third party needs to generate its own RSA key pair, keep the RSA private key, upload the RSA public key and let Tiger Securities check the third party's API request.
* Third parties download the open platform RSA public key for verifying open platform callbacks

### Signature Rules

* Arrange the parameters (except sign) in lexicographical order to form a string, the format of which is shown in the following example.
* The resulting string is then signed by the access party with its own private key.
* Generated signature is assigned with the sign, and passed along with other parameters to the API socket

The api service verifies the parameters and signatures (where the signature algorithm is described below).

Request content needs to be signed in (json format):

```json
{"tiger_id": "20151237", "charset": "UTF-8", "sign_type": "RSA", "version": "1.0", "method": "market_state", "biz_content": "{\"market\": \"US\"}", "timestamp": "2018-11-26 15:31:19"}
```

According to the key, the dictionary is sorted to form a signed string as follows:

```json
biz_content={"market": "US"}&charset=UTF-8&method=market_state&sign_type=RSA&tiger_id=20151237&timestamp=2018-11-26 15:31:19&version=1.0
```

Content used for the post request (json format, including the signature)

```json
{"tiger_id": "20151237", "charset": "UTF-8", "sign_type": "RSA", "version": "1.0", "method": "market_state", "biz_content": "{\"market\": \"US\"}", "timestamp": "2018-11-26 15:33:27", "sign": "tP1NandnEdB7LTiYoMGUF4="}
```

### Signature Algorithm Sample

!!! note
    The signature algorithm code is already encapsulated in the SDK. If you call the SDK directly, you don't need to implement the signature algorithm yourself.

```java tab="Java"
public String rsaSign(String content, String privateKey, String charset) {
    try {
        PrivateKey priKey = getPrivateKey("RSA", new ByteArrayInputStream(privateKey.getBytes()));
        Signature signature = Signature.getInstance("SHA1WithRSA");
        signature.initSign(priKey);
        signature.update(content.getBytes(charset));
      return new String(Base64.encodeBase64(signature.sign()));
    } catch (Exception e) {
        throw new RuntimeException("rsa sign error:", e);
    }
}

public PrivateKey getPrivateKey(String algorithm, InputStream stream) throws Exception {
    KeyFactory keyFactory = KeyFactory.getInstance(algorithm);
    byte[] encodedKey = StreamUtil.readText(stream).getBytes();
    encodedKey = Base64.decodeBase64(encodedKey);
    return keyFactory.generatePrivate(new PKCS8EncodedKeySpec(encodedKey));
}
```

```python tab="Python"
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
Request signature field( Fields are arranged in alphabetical order & stitched)
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
Field signature( Call this method to sign, charset='UTF-8')
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

Signature verification(
Check with Tiger's public key to ensure that the returned content has not been tampered)
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

```C# tab="C#"
using System;
using System.Security.Cryptography;
using System.Text;
namespace ConsoleApplication1
{
    public class Cryptograph
    {
        public static string SignData(string message, RSAParameters privateKey)
        {
            //// The array to store the signed message in bytes
            byte[] signedBytes;
            using (var rsa = new RSACryptoServiceProvider())
            {
                //// Write the message to a byte array using UTF8 as the encoding.
                var encoder = new UTF8Encoding();
                byte[] originalData = encoder.GetBytes(message);

                try
                {
                    //// Import the private key used for signing the message
                    rsa.ImportParameters(privateKey);

                    signedBytes = rsa.SignData(originalData, new SHA1CryptoServiceProvider());
                }
                catch (CryptographicException e)
                {
                    Console.WriteLine(e.Message);
                    return null;
                }
                finally
                {
                    //// Set the keycontainer to be cleared when rsa is garbage collected.
                    rsa.PersistKeyInCsp = false;
                }
            }
            //// Convert the a base64 string before returning
            return Convert.ToBase64String(signedBytes);
        }

        public static bool VerifyData(string originalMessage, string signedMessage, RSAParameters publicKey)
        {
            bool success = false;
            using (var rsa = new RSACryptoServiceProvider())
            {
                var encoder = new UTF8Encoding();
                byte[] bytesToVerify = encoder.GetBytes(originalMessage);

                byte[] signedBytes = Convert.FromBase64String(signedMessage);
                try
                {
                    rsa.ImportParameters(publicKey);

                    success = rsa.VerifyData(bytesToVerify, new SHA1CryptoServiceProvider(), signedBytes);
                }
                catch (CryptographicException e)
                {
                    Console.WriteLine(e.Message);
                }
                finally
                {
                    rsa.PersistKeyInCsp = false;
                }
            }
            return success;
        }
    }
}
```