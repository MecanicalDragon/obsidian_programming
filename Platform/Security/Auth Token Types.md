Authorization token types:

**Bearer**
`Authorization: Bearer <access_token>
This type implies if you have the token it is enough to consider you as one with access.

**Basic**
`Authorization: Basic <base64(username:password)>`
Login and password are encoded in base64. Not safe.

**Digest**
`Authorization: Digest username="user", realm="example", nonce="abc123"`
Uses hashes and [[nonce]] for the password protection.

**HOBA**
`Authorization: HOBA sig="base64sig", keyId="keyid", ...`
HTTP Origin-Bound Authentication, uses cryptographic signatures instead of passwords. Used rarely. 

**AWS Signature**
`Authorization: AWS4-HMAC-SHA256 Credential=..., SignedHeaders=..., Signature=...`
This token is not specified in RFC standards but it is exploited in Amazon. It is very strict and contains data, headers, payload hash, signature, etc. [[HMAC]] means user has a secret key that is hashed with data in every message to ensure authenticity.

Bearer, Basic and Digest are standard token types that are comprehensible to every proxy, framework or library, but you also can always create your own custom types.