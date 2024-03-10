# Networking

**Communication Protocols** that standardize message formats and rules about how these messages should be sent and received are composed of layers. Each layer provides a specific function to the layer above it, but each layer is encapsulated; in other words, it does not need to know how the other layers work.

Two major models describe these layers: the **OSI (Open System Interconnect) model** and the **TCP/IP (Transmission Control Protocol/Internet Protocol) model**.

The TCP/IP model is a simplified implementation of the OSI model - and it has four layers (compared to seven in the OSI model). It defines how data is transmitted from one computer to another, and how a computer should be connected to the internet. Its design allows for high communication reliability through an unreliable network.

Each layer in the TCP/IP model has protocols, which each describe the message formats and rules that pertain to the later.

![source: https://www.nwkings.com/wp-content/uploads/2022/12/Network-Components-33-min-1024x576.png](https://www.nwkings.com/wp-content/uploads/2022/12/Network-Components-33-min-1024x576.png)

Some definitions of the common protocols:

- **IP** - **Internet Protocol** - set of rules for routing packets of data across networks. Each device on the network is assigned an **IP address** which uniquely identifies the device so that other devices on the internet can find it. **IPv4** is the fourth version of the protocol, and it defines an address sa a set of four numbers that range from `0.0.0.0` to `255.255.255.255`. Data is divided into smaller pieces, called **packets**, and sent through the internet. Each packet contains the IP information which directs routers and devices to send the packets to the correct destination. After the packets reach the correct IP destination, a **transport protocol** is then responsible for handling that data (e.g. TCP or UPD below).
- **TCP** - **Transmission Control Protocol** - implemented on top of IP, it is a connection oriented protocol that allows for reliable transmission of data on an unreliable network. A network can become unreliable because packets can be dropped, corrupted, or unordered. TCP solves these problems through retransmission, check-sum, and order checking.
- **UDP** - **User Datagram Protocol** - implemented on top of IP but does not have the reliability and guarantees of TCP. However, UDP has lower overhead and latency compared to TCP, and is useful for applications where lost packets are not essential e.g. video playback or streaming content.
- **HTTP** - **Hyper Text Transfer Protocol** - implemented on top of TCP with the purpose of communicating with a web server that hosts websites and establish a connection with the server. In HTTP, a client sends requests to a web server and receives HTML back as responses.
- **TLS** - **Secure Sockets Layer** - used to authenticate and encrypt connections in networks. Used for secure connections for messaging and email applications for example. When TLS is used on HTTP it is called HTTPS.
