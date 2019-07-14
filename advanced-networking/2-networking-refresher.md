
# The OSI Model: 7 Layers

1. **Physical**
    - Very little intelligence.
    - Uses a defined electrical format (voltage, etc).
    - It is the cabling between
    - Don't know each other (just data on a wire)
    - Ethernet and Wifi are physical layer protocols (but they're totally different)
1. **Data Link**
    - Can talk from machine to machine
    - Talk using MAC address
    - Uses frames to communicate
    - Handles sequencing and flow control (how much data is transferred)
    - Handles data correction (such as if corrupted by the physical layer)
    - Can also re-order frames if needed.
1. **Network**
    - Packes are used here
    - Computers are talking via IP at this point
    - No error checking happenings at this level (ICMP, IP)
1. **Transport**
    - Uses segments (or data segment)
    - Can also handle error control
    - TCP and IP operate at this level
1. **Sesssion**
    - We add the concept of on-going sessions here
    - This is the layer that controls long-lived connections
    - Handles the perception of a connection
    - Handles ports (e.g. UDP and DNS @ 53, TCP for HTTP @ 80)
    - Ports allow multiple services to be addressed on one host (not possible at lower layers)
1. **Presentation**
    - ????
1. **Application**
    - Where the application protocols are added
    - Devices don't know that it's TCP, they just see data over a port


### How does data communicate between layers?

Note:
- Layers don't know whats happening up and down
- Layer 4 talks to layer 4 across computers

**Encapsulation:** How data is wrapped and passed down/up the stack

- Application (Basic Data): `[DATA]`
- Session (Segment) `[[DATA] + TCP HEADER (SRC / DEST PORT)]`
- Network (IP Packet) `[[[DATA] + TCP HEADER (SRC / DEST PORT)] + IP ]`
- DataLink (Ethernet Frame) `[[[DATA] + TCP HEADER (SRC / DEST PORT)] + IP ] + MAC]`
- Physical (Binary) `0101010010`

# IPV4

- IPV4 is the fourth generation of the IP
- IP is _connectionless_, it has no notion of ports, sessions. Simply sends data from point A to point B
- IP is 32 bit binary (4x octet blocks) usually converted to the 256 numbers

**Host vs Network Mask**
- Part represents the network and the host side of the address
- Netmasks distinguish between the host and the network side (represented in decimal or binary)

## Netmasks & Network Size

The netmasks is: The number of bits out of 32 that are given to the network (as opposed to the local host)

Usually `/24`, `/16` or `/8`

Remember: `/3` netmask is twice the size of `/2` is twice the size of `/1` (it doubles).

You _could_ use different network masks, such as: `/22`

- 255.255.255.0 = /24
    - class B network
    - 24 bits for network
    - 8 bits for the host
    - Total space = [2^8 = 256 addresses]
- 255.255.0.0 = /16 (16 bits for network, 16 for the host) [2^16 = 65536 addresses]
    - class B network
    - 16 bits for network
    - 16 bits for the host
    - Total space = [2^16 = 65536 addresses]
- 255.0.0.0 = /8
    - class A network
    - 8 bits for network
    - 24 bits for the host
    - Total space = [2^24 = 16777216 addresses]

### Network / Broadcast Address

-

## Questions

- How are netmasks passed as part of the IP protocol?

