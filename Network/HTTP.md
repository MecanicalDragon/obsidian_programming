**HTTP/0.9** - was the first version of the protocol and had no version. It was extremely simple: 

- Requests consist of a single line and start with the only possible method `GET` followed by the path to the resource. 
- No HTTP headers, meaning that only HTML files could be transmitted.
- No status or error codes: in case of a problem, a specific HTML file with the description of the problem was sent back.
- *TCP connection terminated immediately after the response.*

**HTTP/1.0** - each request must establish its own TCP connection.

- Versioning information is now sent within each request (`HTTP/1.0` is appended to the first line)
- A status code line is also sent at the beginning of the response.
- The notion of HTTP headers has been introduced, both for the requests and the responses.
- With the help of `Content-Type` header, the ability to transmit documents other than plain HTML files has been added.
- `POST` and `HEAD` methods added
- *TCP connection terminated immediately after the response.*

**HTTP/1.1** - allows a single TCP connection to be reused.

- Mandatory `Host` header was specified, providing an ability to host different domains at the same IP address.
- New [[HTTP methods]] introduced (all others)
- New *Continue* status codes ([[HTTP Status Codes#1xx]]) allow clients to send only headers first and learn if a request can be performed to avoid servers refusing unpossessable requests.
- *A TCP connection, being established, can be reused for the consequent requests and responses saving time on handshakes.* That is called a *Persistent connection*. From now the default value of the `Connection` header is `keep-alive`. Keep-alive header can be used to specify timeout and max connections number.
- *Pipelining* was added, allowing to send another request before receiving the answer for the previous one. Limitations are that responses must be received in the same order as requests and particular request\response sendings\receivings are sequential. Hence, if one response fails, all subsequent responses will fail too.
- Chunked responses are now also supported.

**HTTP/2** - a single TCP connection can handle multiple *asynchronous* requests.

- It is a binary protocol rather than text. Transmitted data can no longer be read and created manually.
- *HTTP Streams* introduced. Now it is a multiplexed protocol. *Parallel requests and responses can be handled over the same connection asynchronously*, removing the order and blocking constraints of the HTTP/1.1. But it also has a drawback: if a package is lost for a single request, all streams fail. This problem is known as *head-of-line blocking*.
- Headers compressing. Since headers are often similar among a set of requests, this removes duplication and overhead of data transmitted.
- Ability to reset connections independently allows to close a connection for any reason, thus immediately opening a new one.
- Request prioritization allows the client to set a numeric prioritization in a batch of requests. Thus, we can be explicit in which order we expect the responses, such as getting a webpage CSS before its JS files.
- Server push. To avoid a server receiving lots of requests, the server tries to predict the resources that will be requested soon. So, the server proactively pushes these resources to the client cache.

**HTTP/3** - uses *QUIC* (Quick UDP Internet Connections) protocol.

- This one is built on the top of UDP but can determine and requery missed packages.
- Each Stream is handled independently, so package loss in one stream doesn’t affect other streams.
- *Connection ID* concept implemented, allowing connections to move between IP addresses and network interfaces.