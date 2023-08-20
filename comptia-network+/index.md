
## OSI + TCP/IP Model

- Both models relate to each other. 
- Each layer has a name and number. 

**OSI (older, more detailed)**
7. Application -> API interfaces.
6. Presentation -> Old. Data Format.
5. Session -> TCP/IP, Websockets.
4. Transport -> Packets.
3. Network -> IP Addresses. 
2. Data Link -> Mac Address. Network Cards. Switches.
1. Physical -> Cables.

**TCP/IP (more modern, less detailed)**
4. Application -> 
3. Transport -> Assembly / Dissassembly. 
2. Internet -> Routers + IP.
1. Network Interface -> Physical Cabling. Harware.

## IP Addresses
- Dual stack (runs both IPv4 and IPv6)
- Single stack (runs only IPv4)
- IPv4
  - IPv4 addresses have four address ranges
  - IPv4 addresses have eight address ranges
  - We've worked around IPv6 with private IP and NAT
- IPv6
  - Introduces aggregation (better routing)
- DHCP is dead? (Replaced with NDP?)
- You can use "double colon" shorthand to group 0's
- Two IP Addresses
  - Link local address (Always: `fe80:0000:0000:0000`)
  - Internet Address
- IPv4 = 32 bits
- IPv6 = 128 bits

## Questions

- What is DHCP?
- Why colon separators for IPv6?
- What is a link local address?
- Neighbour discovery protocol?