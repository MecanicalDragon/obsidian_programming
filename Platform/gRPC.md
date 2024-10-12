**Remote Procedure Call** - technology that allows programs to call procedures remotely. Traditionally, it consists of two components: transport protocol and object serialization language (OSL). gRPC uses *HTTP/2* as a transport protocol and *Protocol Buffers* as an OSL.
gRPC determines 4 types of client-server interaction:
- **Unary RPC**. Client sends a request and receives a response, as usually.
- **Server Streaming RPC**. Client sends a request and reads a stream of sequential events from the server.
- **Client streaming RPC**. Client sends a stream of sequential events and receives a response from the server in the end.
- **Bidirectional streaming RPC**. Both client and server write and read streams of sequential events.
