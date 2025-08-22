`192.168.17.10` – ip-address. Each IP address consists of a network-identifying prefix followed by a host identifier. IP address consists of 4 octets.

If the last octet == 0 - this IP address is a network IP. 
If the last octet == 255 - this IP address is a *broadcast IP*, meaning the packet will be sent to all participants of the network. [[DHCP]] may query the network by this IP for participants that has no ip-address or that has a certain ip-address.

In the previous [IPv4](https://en.wikipedia.org/wiki/IPv4) [classful network](https://en.wikipedia.org/wiki/Classful_network) architecture, the top three bits of the 32-bit IP address defined how many bits were in the network prefix:
- Class A: 8 Network prefix bits and 24 host id bits
- Class B: 16/16
- Class C: 24 Network prefix bits and 8 host id bits

That wasn’t scalable; that’s why *Classless Inter-Domain Routing* (**CIDR**) was invented. In addition to IP-address it provides a subnet mask.

`255.255.255.0` – subnet mask. Non-zero numbers mean the number of first-going bits in ip address that determine Subnet ID, the rest bits determine host ID. The selfsame ip address with a subnet mask in a CIDR notation: `192.168.17.10/24`; 24 – number of subnet mask bits.

`0.0.0.0/0` – all TCP/IP addresses on the internet.

Private subnets, that have no access to the internet:
- `10.0.0.0` – `10.255.255.255`
- `127.0.0.0` – `127.255.255.255`
- `172.16.0.0` – `172.31.255.255`
- `192.168.1.0` – `192.168.255.255`

Command `ipconfig` – know your ip config on Windows.
Command `ip addr` or `hostname -I` – know your ip config on Linux.

[Classless inter-domain routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)
