![](http_methods.png)
["fresh enough"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html) is a term implying a set of caching policies in HTTP. These policies include requests with the following headers usage:
- Cache-Control: max-age, immutable, no-cache, must-revalidate, etc.
- Expires
- ETag + If-Match / If-None-Match
- Last-Modified + If-Modified-Since / If-Unmodified-Since

## How to make POST and PATCH requests idempotent

1. Use the preliminarily created unique business identifier in the URL path.
2. Use the *Idempotency-Key* header and transactionally save it in the storage if request processing was successful.
3. Use the *ETag* header. The idea is in the preliminary *GET* request, returning in the *ETag* header something that can be used as a data snapshot of the modifying entity to compare it later during the modifying request with the actual value. If ETag doesn’t guarantee strong consistency, it is prefixed with `W/`. If ETag is required before the modifying request, the *428* status code should be returned.
	1. *If-Match* header makes the server check if the specified ETag value matches the source entity version and return *412* if it doesn’t.
	2. *If-None-Match* header makes the server check if it has entities giving this ETag value and return *304* in this case.
4. *If-Modified-Since* and *If-Unmodified-Since* headers hold a timestamp in the special format, received previously in the *Last-Modified* header of the server’s response for the preemptive *GET* request. A problem with this approach is that the precision of this timestamp is limited to seconds.

[source](https://www.mscharhag.com/api-design/rest-making-post-patch-idempotent) and [source](https://www.mscharhag.com/api-design/rest-concurrent-updates)
