# Servers

The main core networking modules for creating web applications in Node.js:

1. **net/require('net'):** provides the foundation for creating TCP server and clients
2. **dgram/require('dgram'):** provides functionality for creating UDP / Datagram sockets
3. **http/require('http'):** provides a high-performing foundation for an HTTP stack
4. **https/require('https'):** provides an API for creating TLS / SSL clients and servers

| Term     | Description                              |
| -------- | ---------------------------------------- |
| HTTP     | Hypertext Transfer Protocol--An application-layer client-server protocol built on TCP. |
| TCP      | Transmission Control Protocol--Allows communication in both directions from the client to the server, and is built on to create application-layer protocols like HTTP. |
| UDP      | User Datagram Protocol--A lightweight protocol, typically chosen where speed is desired over reliability. |
| Socket   | The combination of an IP address and a port number is generally referred to as a socket. |
| Packet   | TCP packets are also known as segments--the combination of a chunk of data along with a header. |
| Datagram | The UDP equivalent of a packet.          |

> You do not need to create the web server from scratch. There are great framework available such as expressJS.
