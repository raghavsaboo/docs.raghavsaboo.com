## DNS

**Domain Name System (DNS)** is used to resolve **domain names** (such as www.google.com) to IP addresses. 

The translation from a domain name to IP address occurs through a process called **DNS resolution** or **DNS lookup**. There are four types of **DNS Name Servers** used for DNS resolution, and they process a lookup request in the following order:

1. **Resolver Name Server (DNS Resolver)** - interacts with clients such as web browsers and is the initial server in a DNS lookup. The resolver either directs the request to the root name server or returns cahced data. Since there might be clients that request the same domain name, the resolver caches the IP addres of the domain and returns it directly.
2. **Root Name Server** - responds to the resolver's request by directing the request to a TLD server based on the extension of the domain name (.com, .org, .net)
3. **TLD Name Server** - a TLD (Top Level Domain) server holds information for all the domain names that have the same extension. For example the `.com` TLD server holds the information about which authoritative name server holds the DNS records for the domain name.
4. **Authoritative Name Server** - holds the actual DNS record which contains the IP address of the server that corresponds to the domain name.

![source: https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png](https://d1.awsstatic.com/Route53/how-route-53-routes-traffic.8d313c7da075c3c7303aaef32e89b5d0b7885e7c.png)