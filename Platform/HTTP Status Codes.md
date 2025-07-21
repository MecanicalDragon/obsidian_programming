## 1xx
**100 Continue** – intermediate response, means request has been received by the server successfully. Client can continue sending requests or ignore it.

**101 Switching Protocol** – this code is being sent as a response when the client sends request with *Upgrade* header with the desired protocol specified in it. The server switches to it and sends its name back in the same *Upgrade* header. Usually is not used, because server can switch to incompatible for the client protocol version.

**102 Processing** – server has received the request and is processing it. Processing is not finished yet.

**103 Early Hints** – response contains list of resources that can be requested beforehand while server processes this request.
## 2xx
**200 OK** – request has been handled successfully.

**201 Created** – new resource has been created. Response contains *Location* header with URI of the created resource.

**202 Accepted** – processing has started but may be not finished yet.

**203 Non-Authoritative Info** – response has been provided by not original source (cache for instance).

**204 No Content** – No content in response body; headers can be provided.

**205 Reset Content** – client must reset document view that has sent this request; doesn’t provide any content.

**206 Partial Content** – success of [Partial GET](https://ru.wikipedia.org/wiki/HTTP#%D0%A7%D0%B0%D1%81%D1%82%D0%B8%D1%87%D0%BD%D1%8B%D0%B5_GET) request. *Content-Range* header with the content part included in the response must be specified.

**207 Multi-Status** – result of several independent operations wrapped in *multistate xml document*.

**208 Already Reported** – successful response status code that is used in a *207 Multi-Status* response to save space and avoid conflicts if the same entity is supposed to be returned several times in different parts of Multi-Request.
## 3xx
**300 Multiple Choices** – multiple MIME types, languages or something else are available. Request has more than one possible response. The user-agent or the user should choose one of them.

**301 Moved Permanently** – the browser should automatically request the resource at the URL in the *Location* header, redirecting the user to the new page. Access method to specified resource must be *GET*.

**302 Found (Moved Temporarily)** – `Deprecated` – tells the client to browse to another URL. *Location* header should be provided. The HTTP/1.0 specification required the client to perform a redirect with the same method, but browsers implemented 302 redirects with *GET*. Therefore, HTTP/1.1 added status codes 303 and 307 to distinguish between these two behaviors.

**303 See Other** – precepts to browse to URI from *Location* header, that should be provided, with *GET* method. This response code is often sent back as a result of PUT or POST methods so the client may retrieve a confirmation, or view a representation of a real-world object.

**304 Not Modified** – says that resource has not been modified since time, specified in request’s *If-Modified-Since* or *If-None-Match* headers, therefore there is no necessity to respond with it once more, and client can use previously received data.

**305 Use Proxy** – `Deprecated` – precepts to use for request proxy, specified in the *Location* header.

**306 Switch Proxy** – `Deprecated` – precepts to use for following requests proxy, specified in *Location* header.

**307 Temporary Redirect** – browse to *Location* header URI with the same request method and the body in the redirected request.

**308 Permanent Redirect** – browse to *Location* header URI with the same request method and the body in the redirected request.

## 4xx
**400 Bad Request** – invalid or corrupted data received from client.

**401 Unauthorized** – *WWW-Authenticate* header required; `ROLE_1, ROLE_2` should be denoted there.

**402 Payment Required** – unused.

**403 Forbidden** – client has no permissions to access.

**404 Not Found** – resource not found.

**405 Method Not Allowed** – *Allow* header with accessible methods list required.

**406 Not Acceptable** – server could not produce a response matching the list of acceptable values defined in the request's proactive content negotiation headers (*Accept*, *Accept-Encoding*, *Accept-Language*)

**407 Proxy Authentication Required** – request lacks valid authentication credentials for the proxy server that sits between the client and the server with access to the requested resource.

**408 Request Timeout** – connection time expired.

**409 Conflict** - indicates that the request could not be processed because of conflict in the current state of the resource.

**410 Gone** - indicates that the resource requested is no longer available and will not be available again. This should be used when a resource has been intentionally removed and the resource should be purged.

**411 Length Required** – the request did not specify the length of its content, which is required by the requested resource.

**412 Precondition Failed** – the server does not meet preconditions that the requester put in the request header fields. Sent when the condition defined by the *If-Unmodified-Since* or *If-Match* headers is not fulfilled.

**413 Payload Too Large** - the request is larger than the server is willing or able to process.

**414 URI Too Long** – provided URI is too long for the server to process.

**415 Unsupported Media Type** – request entity has a media type which the server or resource does not support.

**428 Precondition Required** – server requires the request to be conditional. Intended to prevent the 'lost update' problem. Typically, *If-Match* is missing.

**429 Too Many Requests** – user sent too many requests in a given amount of time. Intended for use with rate-limiting schemes. May return [Retry-After](https://developer.mozilla.org/ru/docs/Web/HTTP/Headers/Retry-After) header.

**431 Request Header Fields Too Large** – can be used when the total size of request headers is too large or when a single header field is too large.

## 5xx
**500 Internal Server Error** – generic error message given when an unexpected condition was encountered and no other specific messages are suitable.

**501 Not Implemented** – server either does not recognize the request method or it lacks the ability to fulfill the request.

**502 Bad Gateway** – server acted as a gateway or proxy and received an invalid response from the upstream server.

**503 Service Unavailable** – temporary server cannot handle the request.

**504 Gateway Timeout** – server is a gateway or proxy and did not receive a timely response from the upstream server.

**505 HTTP Version Not Supported** –  server does not support HTTP protocol version used in the request.
