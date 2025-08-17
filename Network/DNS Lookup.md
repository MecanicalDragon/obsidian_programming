DNS Lookup is the first step of internet connection establishment.

**Forward DNS** is a request that is used to obtain an IP address by searching the domain. This follows the standard DNS query journey when the user types in a web page or sends an email and is provided with the related IP address.

**Reverse DNS** is the exact opposite of forward DNS. It is a lookup request that is used to obtain the domain name related to an IP address. Reverse lookups are typically used by email servers to ensure that the servers they are receiving messages from are valid.

The [[DNS]] server keeps different record types for every domain. For each domain there may be multiple records of each type. During the DNS lookup server picks one of them according to its lookup policy. This is some sort of load balancing. Record types:
- **A** - Address or IPv4 DNS records, these store IP addresses for domain names.
- **AAAA** - Address v6 or IPv6 DNS records, same as A records but store IPv6 IP addresses.
- **PTR** - Pointer to reverse DNS records. This is the opposite of A or AAAA types and is used to turn IP addresses into a hostname.
- **NS** - Nameserver DNS records store the authoritative nameserver for a domain name.
- **CAA** - Certificate Authority Authorization DNS records are used to store which certificate authorities are allowed to issue certificates for the domain.
- **CNAME** - Canonical Name or sometimes known as Alias records are used to point to other DNS records. Often used for subdomains like www.
- **MX** - Mail Exchanger DNS records are used to store which email servers are responsible for handling email for the domain name.
- **SOA** - Start of Authority DNS records store meta details about a domain name such as the administrator contact email address and when the domain last had changes made to its DNS configuration.
- **SRV** - Service DNS records store protocol and port numbers for services by the domain name, for example VoIP or chat server.
- **TXT** - Text records are used to store notes as DNS records, however they are typically used to store configuration settings for various services like SPF records which are used to define which email servers are allowed to send email from the domain or verification codes for some webmaster tools.

