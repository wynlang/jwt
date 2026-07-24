# jwt - Official Wyn Package

JSON Web Token encode / decode / verify. Pure Wyn using the built-in
`Crypto` and `Encoding` modules.

## Install

```bash
wyn pkg install github.com/wynlang/jwt
```

## Usage

```wyn
import jwt

var token = jwt.Jwt_encode("{\"sub\":\"user123\",\"exp\":9999999999}", "my-secret")
var payload = jwt.Jwt_decode(token)          // "{\"sub\":\"user123\",...}"
var ok = jwt.Jwt_verify(token, "my-secret")  // 1 = valid, 0 = invalid
```

Public API:

- `Jwt_encode(payload, secret) -> string` — HS256, returns `header.payload.signature`
- `Jwt_decode(token) -> string` — returns the decoded payload segment
- `Jwt_verify(token, secret) -> int` — 1 if the signature matches, else 0
- `base64url_encode(s) / base64url_decode(s)` — URL-safe base64 helpers

## ⚠️ Non-standard signature encoding — NOT RFC-7519 interoperable

This package signs with `base64url(hex(hmac_sha256(...)))`, because the
built-in `Crypto.hmac_sha256` returns a hex string (64 chars), not raw
bytes. A standards-compliant JWT encodes the *raw* HMAC bytes as
`base64url`. As a result:

- Tokens produced here are **self-consistent** (they encode/verify
  round-trip within this library), but they are **NOT interoperable** with
  RFC-7519 / real JWT libraries (jsonwebtoken, PyJWT, jose, etc.). Those
  libraries will reject the signature.
- Use this for signing/verifying tokens *within Wyn programs only*. Do not
  hand these tokens to, or accept them from, external JWT verifiers.

A raw-bytes fix awaits a raw-output HMAC in the runtime `Crypto` module.

## Test

```bash
wyn run tests/test_jwt.wyn
```
