# Video References

- [OAuth 2.0 and OpenID Connect (in plain English)](https://www.youtube.com/watch?v=996OiexHze0 "OAuth 2.0 and OpenID Connect (in plain English)") (this is the best one, also the longest)
- [How to Hack OAuth](https://www.youtube.com/watch?v=aU9RsE4fcRM "How to Hack OAuth")
- [An Illustrated Guide to OAuth and OpenID Connect](https://www.youtube.com/watch?v=t18YB3xDfXI "An Illustrated Guide to OAuth and OpenID Connect")
- [Vulnerabilities of mobile OAuth 2.0 by Nikita Stupin, Mail.ru](https://www.youtube.com/watch?v=vjCF_O6aZIg&ab_channel=scrt.insomnihack) (there's a topic on PKCE here)

# Identity Use Cases

| Case                    | Type           | Implementation |
| ----------------------- | -------------- | -------------- |
| Simple login            | Authentication | oidc           |
| SSO                     | Authentication | oidc           |
| Mobile login            | Authentication | oidc           |
| Delegated authorization | Authorization  | OAuth 2.0      |

# Introduction
## Roles

- resource owner
- resource server
- client
- authorization server

## Protocol Flow

![[Pasted image 20240915204549.png]]

## Authorization Grant

- authorization code
- implicit
- resource owner credentials
- client credentials
- extensible mechanisms

## Tokens

- access token: credentials to access protected resources, used by resource servers
- refresh token (optional): credentials used to obtain access tokens, used by authorization servers

![[Pasted image 20240915211903.png]]

# Clients

## Types

- confidential
- public
- web application
- user-agent-based application
- native application

## Authentication

- HTTP Basic authentication, e.g; Authorization Basic czZCaGRSa3F0Mzo3RmpmcDBaQnIxS3REUmJuZlZkbUl3
- client credentials (client id and client secret)

# Protocol Endpoints

## Authorization server

- Authorization endpoint
- Token endpoint

## Client

- redirection endpoint

# Obtaining Authorization

## Authorization Code Grant

### Authorization Request

- response_type: REQUIRED, value must be "code"
- client_id: REQUIRED
- redirect_uri: OPTIONAL
- scope: OPTIONAL
- state: RECOMMENDED (for CSRF prevention)

### Authorization Response

- code: REQUIRED (this is the authorization code, and must be short lived, recommended maximum life time is 10 mins and must only be used once)
- state: REQUIRED, if part of authorization request

### Access Token Request

- grant_type: REQUIRED, value must be "authorization_code"
- code: REQUIRED (the authorization code received from authorization server)
- redirect_uri: REQUIRED, if part of authorization request
- client_id: REQUIRED, if client not authenticated

### Access Token Response

```json
 {
   "access_token":"2YotnFZFEjr1zCsicMWpAA",
   "token_type":"example",
   "expires_in":3600,
   "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
   "example_parameter":"example_value"
 }
```

## Implicit Grant

### Authorization Request

- response_type: REQUIRED, value must be "token"
- client_id: REQUIRED
- redirect_uri: OPTIONAL
- scope: OPTIONAL
- state: RECOMMENDED (for CSRF prevention)

### Access Token Response

- access_token: REQUIRED
- token_type: REQUIRED
- expires_in: RECOMMENDED
- scope: OPTIONAL, if identical with the requested scope, else REQUIRED
- state: REQUIRED, if part of authorization request
- MUST NOT include refresh token

## Resource Owner Password Credentials

## Access Token Request

- grant_type: REQUIRED, value must be "password"
- username: REQUIRED
- password: REQUIRED
- scope: OPTIONAL

### Access Token Response

```json
 {
   "access_token":"2YotnFZFEjr1zCsicMWpAA",
   "token_type":"example",
   "expires_in":3600,
   "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
   "example_parameter":"example_value"
 }
```

## Client Credentials Grant

### Access Token Request

- grant_type: REQUIRED, value must be "client_credentials"
- scope: OPTIONAL

### Access Token Response

```json
 {
   "access_token":"2YotnFZFEjr1zCsicMWpAA",
   "token_type":"example",
   "expires_in":3600,
   "example_parameter":"example_value"
 }
```

## Issuing Access Tokens

### Response Headers

- Content-Type: application/json;charset=UTF-8
- Cache-Control: no-store
- Pragma: no-cache

### Refresh Access Token Request

- grant_type: REQUIRED, value must be "refresh_token"
- refresh_token: REQUIRED
- scope: OPTIONAL

RFCs
- [RFC-6749 The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749) (Proposed Standard - October 2012)
- [RFC-7009 OAuth 2.0 Token Revocation](https://datatracker.ietf.org/doc/html/rfc7009)
- [RFC-7662 OAuth 2.0 Token Introspection](https://datatracker.ietf.org/doc/html/rfc7662)