gRPC is an [[RPC]] developed by google that uses *HTTP/2* as a transport protocol and *Protocol Buffers* as an OSL.
gRPC determines 4 types of client-server interaction:
- **Unary RPC**. Client sends a request and receives a response, as usually.
- **Server Streaming RPC**. Client sends a request and reads a stream of sequential events from the server.
- **Client Streaming RPC**. Client sends a stream of sequential events and receives a response from the server in the end.
- **Bidirectional Streaming RPC**. Both client and server write and read streams of sequential events.
