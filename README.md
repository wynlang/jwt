# jwt — Official Wyn Package

JSON Web Token encode/decode. Pure Wyn using built-in Crypto module.

## Install

```bash
wyn pkg install github.com/wynlang/jwt
```

## Usage

```wyn
var header = base64url_encode("{\"alg\":\"HS256\",\"typ\":\"JWT\"}")
var payload = base64url_encode("{\"sub\":\"user123\",\"exp\":9999999999}")
var sig = Crypto.hmac_sha256("secret", header + "." + payload)
var token = header + "." + payload + "." + sig
```
