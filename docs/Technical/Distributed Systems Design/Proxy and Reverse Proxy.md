# Proxy and Reverse Proxy

## Proxy / Forward Proxy
A **proxy** (also called a proxy server or forward proxy) is a server that acts as an intermediary between a client and a server. A proxy server can regulate traffic according to policies and security features. **Usually implemented on the client side**.

Uses of a proxy include:

1. enforcing security policies on a shared network
2. hiding the origin of a client's data packets and requeusts by masking a client's IP address from the receiving server

## Reverse Proxy
Accepts requests from clients and forwards them to other servers. **Usually implemented on the server side** and acts as a gateway to protect the internal server side system from being exposed to the outside world.

A reverse proxy typically sits in front of web servers and forwards requests to those web servers, which then subsequently process those requests or forward them to backend servers. Additionally, security policies and features can be implemented at this single point to protect all web and backend servers, adding anonymity and security for the entire system without having to implement policies at all servers.

Reverse proxies also perform [**load balancing**](./Load%20Balancing.md), which is distributing traffic across multiple servers.