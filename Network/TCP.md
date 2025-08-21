**TCP** – Transmission Control Protocol. TCP establishes a connection between 2 participants during a *handshake* and ensures some guarantees for this connection like below:
- every package will be delivered without loss or duplication.
- out-of-order packages will not be processed out of order.
- transmission velocity is under control allowing to reduce it if recipient can't maintain it.
- congestion control algorithms protect the network from overwhelming with traffic.
To provide such guarantees TCP assign numbers to every package and controls the order of packages processing and delivery confirmation. To achieve it, *TCP handshake* is necessary.
### TCP Handshake

TCP Handshake happens after [[DNS lookup]] and before [[TLS]], and takes approximately 50 ms. Connection can be terminated independently by each side of the connection.

TCP uses a *three-way handshake* to establish a reliable connection. The connection is full duplex, and both sides synchronize (SYN) and acknowledge (ACK) each other. The exchange of these four flags is performed in three steps: SYN, SYN-ACK, and ACK.

The client chooses an initial sequence number, set in the first SYN packet, and sends it to the server. The server, after it receives a SYN message, also chooses its own initial sequence number, set in the SYN/ACK packet, and sends it back to the client. During the final step the client acknowledges the server's response and sends its own ACK back. After that they start the actual data transfer. This mechanism allows client and server to acknowledge each other's sequence numbers. Incrementing these numbers on both sides and comparing it with received package numbers allows to detect missing or out-of-order segments.

Likewise *TCP Handshake* is required for connection establishment, to close the connection both sites have to do it implicitly during a *four-way handshake*:
- `A → B: FIN`
- `B → A: ACK`
From now on only `B` can send data to `A`. This state is called *half-close*.
- `B → A: FIN`
- `A → B: ACK`

Also, `RST` signal can be sent by any site to reset the connection immediately.
