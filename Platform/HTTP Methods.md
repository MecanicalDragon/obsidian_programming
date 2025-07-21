![](http_methods.png)
["fresh enough"](https://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)

## How to make POST and PATCH requests idempotent

1. Use the preliminarily created unique business identifier in the URL path.
2. Use the *Idempotency-Key* header and transactionally save it in some storage if request processing was successful.
	1. How to deal with requests that have not been completed successfully? What to return? How should clients handle this situation?
	2. What to return if the server receives a request with an already known idempotency key?
3. Use the *ETag* header. The idea is in the preliminary *GET* request, that returns in the ETag header something that can be used as a data snapshot of the modifiable entity to compare it later during the modifying request with the current value. If ETag doesn’t guarantee strong consistency, it is prefixed with `W/`. If ETag is required before modifying the request, the *428* status code should be returned.
	1. *If-Match* header makes the server check if the specified ETag value matches the source entity version, return *412* if not.
	2. *If-None-Match* header makes the server check if it hasn't got ETags with specified value, and return *304* if it has.
4. *If-Unmodified-Since* header holds timestamp in a special format, received previously in *Last-Modified* header of server response for *GET* request. A problem with this approach is that the precision of this timestamp is limited to seconds.

[source](https://www.mscharhag.com/api-design/rest-making-post-patch-idempotent) and [source](https://www.mscharhag.com/api-design/rest-concurrent-updates)
