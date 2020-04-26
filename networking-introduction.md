# Networking Introduction

## OSI Model

Open Systems Interconnection (OSI) Model is a standardised model that demonstrates the theory of computer networking.

OSI layers:

1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

### Application layer (7)

Provides networking options to applications running on the computer. Passes data to presentation layer.

### Presentation layer (6)

Receives data from application layer. In a format most applications can understand. Translates application traffic into standardised format. Handles encryption and compression.  Passes data to session layer.

### Session layer (5)

When it receives data from presentation layer, the session layer tries to establish connections to other computers. Once established, it maintains the connection. If it cannot establish a connection, it handles errors.

When session layer has established a connection, it passes data to the transport layer.

### Transport layer (4)

Transport layer chooses protocol:

1. UDP (User Datagram Protocol)
2. TCP (Transmission Control Protocol)

Once protocol is selected, it breaks data down into 'datagrams' (UDP) and 'segments' (TCP).

### Network layer (3)

Network layer is responsibile for locating the destination of your request. Uses IP to figure out the best route to destination. This is referred to as 'logical addressing'

### Data Link layer (2)

Data Link layer focuses on physical addressing of data transmission. Takes the packet from network layer (which has an IP already), and adds a physical MAC address to it.

MAC (Media Access Control) is burnt into the NIC (Network Interface Card) by the manufacturer and cannot be changed, but you can spoof it.

When receiving data, the data link layer checks to ensure the data hasn't bee corrupted.

### Physical layer (1)

The physical layer is your computer. Converts from and to binary data before sending and receiving.

## Encapsulation

As data is passed through each layer, more information is added to the start of the transmission.

e.g. The header added by the Network layer would include things like the source and destination IP addresses.

## TCP/IP model

Older than OSI model. Consists of 4 layers:

1. Application
2. Transport
3. Internet
4. Network Interface

When compared to OSI model

1. Application (Application, Presentation, Session)
2. Transport (Transport)
3. Internet (Network)
4. Network Interface (Data Link, Physical)

OSI model is easier to learn the theory of networking, but other than that they are very similar.

The process of encapsulating and de-encapsulating are the same.

## Network tools

- `ping`
- `whois`
- `traceroute`
- `dig`