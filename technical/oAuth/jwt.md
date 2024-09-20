JOSE Header
```bash
{"typ":"JWT", "alg":"HS256"}

# encoded JOSE Header, base64url, UTF-8, CRLF
eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9
```

JWT Claims Set
```bash
{"iss":"joe", "exp":1300819380, "http://example.com/is_root":true}
  
# encoded JWS Payload, base64url, UTF-8, CRLF
eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ
```

JWS Signature
```bash
# JWS Signing Input
eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ

# encoded JWS Signature, HMAC SHA-256, JWK key, base64url, UTF-8, CRLF
dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

JWT
```bash
# concatenating together the encoded values with '.'
eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ.dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

Registered Claim Names
- iss (Issuer)
- sub (Subject)
- aud (Audience)
- exp (Expiration Time)
- nbf (Not Before)
- iat (Issued At)
- jti (JWT ID)

RFCs
- [RFC-7519 JWT](https://datatracker.ietf.org/doc/html/rfc7519) (Proposed Standard - May 2015)
- [RFC-7515 JWS](https://datatracker.ietf.org/doc/html/rfc7515) (Proposed Standard - May 2015)
- [RFC-7517 JWK](https://datatracker.ietf.org/doc/html/rfc7517) (Proposed Standard - May 2015)
- [RFC-7516 JWE](https://datatracker.ietf.org/doc/html/rfc7516) (Proposed Standard - May 2015)
- [RFC-9068 JWT Profile for OAuth 2.0 Access Tokens](https://datatracker.ietf.org/doc/html/rfc9068) (Proposed Standard - October 2021)