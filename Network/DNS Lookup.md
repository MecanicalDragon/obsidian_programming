DNS Lookup is the first step of internet connection establishment.

**Forward DNS** is a request that is used to obtain an IP address by searching the domain. This follows the standard DNS query journey when the user types in a web page or sends an email and is provided with the related IP address.

**Reverse DNS** is the exact opposite of forward DNS. It is a lookup request that is used to obtain the domain name related to an IP address. Reverse lookups are typically used by email servers to ensure that the servers they are receiving messages from are valid.

The DNS server keeps different record types for every domain. For each domain there may be multiple records of each type. During the DNS lookup server picks one of them according to its lookup policy. This is some sort of load balancing. Record types:
- **A** - Address or IPv4 DNS records, these store IP addresses for domain names.
- **AAAA** - Address v6 or IPv6 DNS records, same as A records but store IPv6 IP addresses.
- **CAA** - Certificate Authority Authorization DNS records are used to store which certificate authorities are allowed to issue certificates for the domain.
- **CNAME** - Canonical Name or sometimes known as Alias records are used to point to other DNS records. Often used for subdomains like www.
- **MX** - Mail Exchanger DNS records are used to store which email servers are responsible for handling email for the domain name.
- **NS** - Nameserver DNS records store the authoritative nameserver for a domain name.
- **PTR** - Pointer to reverse DNS records. This is the opposite of A or AAAA types and is used to turn IP addresses into a hostname.
- **SOA** - Start of Authority DNS records store meta details about a domain name such as the administrator contact email address and when the domain last had changes made to its DNS configuration.
- **SRV** - Service DNS records store protocol and port numbers for services by the domain name, for example VoIP or chat server.
- **TXT** - Text records are used to store notes as DNS records, however they are typically used to store configuration settings for various services like SPF records which are used to define which email servers are allowed to send email from the domain or verification codes for some webmaster tools.

**Domain Name System** (DNS) is a series of servers located all around the world which store the configuration information of a domain name to provide an ability for TCP/IP users to convert a domain name into an IP address. There are 4 different types of DNS servers involved in a DNS lookup. Each DNS server type has a different role to play and may not all be required under certain circumstances.
- **Recursive Resolver** - This is the DNS server that the computer or device communicates with. This DNS server is typically issued to the client automatically by service provider and is geographically located nearby in order to return results as fast as possible. This server caches DNS record data in order to speed up future DNS lookup requests.
- **Root Nameserver** - This one is responsible for returning the IP address of the *TLD* nameserver. For example, when resolving `example.com`, the root name server will return the IP address of the TLD name server responsible for `.com` domain names.
- **Top-Level Domain Nameserver** - TLD name server is responsible for returning the authoritative name servers for all domains under the TLD it is responsible for. The `.com` TLD name server will return results for `example.com` but not `example.org`.
- **Authoritative Nameserver** - This DNS server stores the DNS configuration data of a domain name.

As an example of the flow of events when performing a DNS lookup, this is the order of events that will happen when you request a URL to visit a website like `example.com` in your web browser.
1. A user types the URL `example.com` into their web browser.
2. Browser first checks its own cache, then checks OS cache for the entry presence.
3. If there’s no required information, the computer sends a request to the *recursive resolver*.
4. The recursive resolver checks its cache for the requested info, then requests the *root nameserver* which provides the address of the *TLD* nameserver responsible for `.com` domain names.
5. When the root nameserver responds, the recursive resolver sends a request to the `.com` TLD nameserver which provides the address of the *authoritative nameserver* responsible for the `example.com` domain.
6. When the TLD nameserver responds, the recursive resolver requests the authoritative nameserver responsible for `example.com` which provides the DNS records requested.
7. The authoritative nameserver returns results to the recursive resolver.
8. The recursive resolver returns DNS records containing the IP address to the browser.
9. The browser makes a request directly to the IP address of the server hosting the website.

![[dns.png]]
