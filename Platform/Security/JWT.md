**JSON Web Token** – technique that describes how to store user information on the user side. Advantages:
- Conforms RESTful architecture style.
- No need to store session data on the server side
- Token can be issued by dedicated service and used in other services
- Token can store additional user information
- Single token can provide an access to multiple services

Token consists of 3 parts, separated with a dot: headers, payload, and signature. First two elements are JSON objects of predefined structure encoded in Base64, third one is calculated based on first two parts, but may be omitted if unsigned JWT is used.

**Headers** contain token description and have only one mandatory field: `alg`, which holds the name of the encoding algorithm. If the token is unsigned, the key holds value `none`. Other keys are `typ` (token type – used when a token is jumbled with other JOSE headers containing objects, otherwise must be `JWT`) and `cty` (content type – if the token has no unregistered custom keys, its value must be `JWT`). If a token is signed, headers may contain additional keys: `jku`, `jwk`, `kid`, `x5u`, `x5t`, `x5c`, `crit`.

**Payload** section is used for user payload. Keys here are called *claims*. Also, payload can contain optional service keys:
- `iss`. Unique id or URI of token generator site.
- `sub`. Unique id or URI of token subject - site (user), whose info is encoded in the token payload.
- `aud`. Array of unique ids or URIs of this token consumers. If the token receiver can’t find itself here it must ignore the token.
- `iat`. Unix Time of token creation.
- `nbf`. Unix Time of moment when token becomes valid.
- `exp`. Unix Time of token expiration.
- `jti`. JWT unique ID.

**Signature** is just a combination of headers, payload and secret salt, encoded with specified in headers algorithm. The hashed string can be verified by any service that knows a sail. This approach doesn’t secure the data but provides impossibility to substitute it. Also, signature can be an encrypted data that is encrypted with symmetric or asymmetric encryption. Asymmetric algorithm usage implies that the auth server exposes a public key to other services to allow them to validate JWT themselves.

For a web-app authentication two tokens are used:
- **Access Token** – used for authentication. It is reusable and has a short lifespan.
- **Refresh Token** – allows a client to request a new access token when it expires. It is disposable and has a long lifespan.

**Workflow**:
1. Client authenticates in application (with login and password, for example).
2. Server sends access and refresh tokens to the client.
3. Client uses access token in requests. Server validates token.
4. 4. When the access token expires, the client sends a refresh token, and the server generates a new token pair.
5. If the refresh token expires, the client must authenticate again.

**Vulnerabilities**:
- [[XSS]]
- [[CSRF]]

---
[OWASP Top 10](https://owasp.org/www-project-top-ten/)

[A bit more about attacks](https://habr.com/ru/post/502702/)
[Access+Refresh](https://habr.com/ru/company/Voximplant/blog/323160/)
[JWT usage](https://habr.com/ru/post/340146/)
[Refresh Token](https://habr.com/ru/post/466929/)
