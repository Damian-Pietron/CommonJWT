## Overview
- JWT - JSON Web Token - compact URL safe way to transmit information between two parties as a signed JSON object. It can verify that one who claims something is true or not.
- Used for auth and aut in web apps and API's

## JWT
- Three parts:
	- Header:
		- Token type and signing algorithm
	- Payload
		- Contains statements about the user or token, such as User ID, roles and expiry time.
        - it consist of:
        
	- Signature
		- Signing the header and the payload with a [[secret key]]

- BaseURL encoded JSON - first two parts. 

Payload is encoded - not encrypyed - so secrets should be passed by it 
revocation needs checking if certain JWT token is valid, 

## Header:

header:    {"alg": "<ALGORITHM>", "typ": "JWT"}


## Payload:

### Registered claims (RFC 7519)

| Claim | Full name | Meaning | Example |
|-------|-----------|---------|---------|
| `sub` | Subject | Who the token is about (usually the user ID) | `"37"` |
| `iss` | Issuer | Who created and signed the token | `"auth.myapp.com"` |
| `aud` | Audience | Who the token is intended for (which service or API) | `"api.myapp.com"` |
| `iat` | Issued At | When the token was created (Unix timestamp) | `1762500000` |
| `nbf` | Not Before | The token is not valid before this time | `1762500000` |
| `exp` | Expiration | The token is not valid after this time | `1762500900` |
| `jti` | JWT ID | A unique ID for this specific token | `"a1b2c3d4"` |

## Example payload

```json
{
  "sub": "42",
  "iss": "auth.myapp.com",
  "aud": "api.myapp.com",
  "iat": 1762500000,
  "nbf": 1762500000,
  "exp": 1762500900,
  "jti": "a1b2c3d4"
}
```

## Signature:

signature: <ALGORITHM>(secret, base64url(header) + "." + base64url(payload))

## How does HS256 work:

1. Split the token into three parts
2. Take the header and payload exactly as received (in Base64URL)
3. Compute your own signature:
   expected = HMAC_SHA256(secret, header + "." + payload)
4. Compare expected with the signature from the token
5. Match → the token is authentic. Mismatch → reject it.

## constant-time comparison 

Expected signature and signature from a token are compared element by element, so each element takes precisely same time to validate

