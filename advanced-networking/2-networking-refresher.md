
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

## Understanding Subnet Masks (& Network Size)

The netmasks is: The number of bits out of 32 (8 x 4) that are given to the network.

Usually subnet masks are: `/24`, `/16` or `/8` (but don't have to be these are your class A,B,C).

Remember: A `/3` netmask gives internal addresses double the space of a `/2`. Which in turn gives double the space of a `/1` netmask. This occurs because you're giving up one extra bit (which creates a X^2 increase in address range to your local network).

You _could_ use different network masks, such as: `/22`

### Class A,B,C networks

### 255.255.255.0 = /24

- class C network (3rd 8bit block)
- 24 bits for network
- 8 bits for the host
- Total space for host = [2^8 = 256 addresses]

#### 255.255.0.0 = /16
- class B network (2nd 8 bit block)
- 16 bits for network
- 16 bits for the host
- Total space for host = [2^16 = 65536 addresses]

#### 255.0.0.0 = /8
- class A network (1st 8bit block)
- 8 bits for network
- 24 bits for the host
- Total space for host = [2^24 = 16777216 addresses]

### Dividing up subnets

If you wanted to work with logically divide a network you can use subnets. To have multiple subnets you can simply chop a subnet in half (and so on).

e.g. if you're given `10.0.0.0` —> `10.0.0.256` with a /24 subnet mask you could chop it up into:

**4 x /25 addresses**

Simply divide in two:

- `10.0.0.0` —> `10.0.0.127`
- `10.0.0.128` —> `10.0.0.255`

**2 x /26 addresses**

Or divide in four:

- `10.0.0.0` —> `10.0.0..63`
- `10.0.0.64` —> `10.0.0.127`
- `10.0.0.128` —> `10.0.0.192`
- `10.0.0.192` —> `10.0.0.255`

You can do this subnet splitting all the way down ( 🐢🐢🐢 )

### Route Tables (Supernetting)

- A route table links networks to one another (next hop)
- You can hardcode _static_ IP's to the route table or you can use _dynamic_ addressing (through various algorithms)
- If address not found it goes to default route (0.0.0.0) and hops out of the network
- *Supernetting* allows multiple route table entries to be grouped together to avoid duplicated entries

### Network Address Translation

- A NAT maps publically available addresses to internal addresses
- Allows you to share a public IP amongst many internal IP's
- Home routers use NAT to link your network (local) to public

### VLAN

- Logically group together machines on a network so they can communicate across a network that has _physical_ access to all other machines
- VLAN's are implemented in the packet routing devices (such as switches or trunks)
- VLANS ensure that only certain computers can talk to each other

## Questions

- How are netmasks passed as part of the IP protocol?
- What is the broadcast / network address?
- Why are broadcast / network addresses the same IP's?
- Why would you want more than 1 public IP if you can use NAT GW's?
- What are the "two bits" at the start of a network range (when subnetting)
- Why are route tables needed?
- How does supernetting really work?
- How does NAT translation work (where is the tag stored in the frame/packet)?
- Is a trunk a physical device? Why use one instead of a switch etc?
