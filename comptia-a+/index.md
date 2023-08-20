
## To Watch

- [] RAM
- [] CPU
- [] Firmware
- [] Storage

## RAM

- 

## IP / TCP

- Four characters 0-255
- Addresses never end with 0 or 255
- Subnet Mask (Tells you if it's in your LAN)
  - Router usually has a `.1` 
  - Most subnet masks are `255.255.255.0`
- Default Gateway
- Ping
- Purpose of IP
  - Identify with LAN you're part of
  - Gives you a unique host ID
  - Class E -> Reserved
  - Class D -> Multicast address (?)
  - Class C (last number is locked) e.g. `210.11.12.x`
  - Class B (last two numbers are locked) `172.16.x.x`
  - Class A (last three numbers are locked) `6.x.x.x`

## NAT

- Gateway Router (2 connections to LAN and ISP)
- Converts external public IP to LAN IP
- Anything inside the LAN is not visible to the internet

## CPU

- Two main manufacturers: Intel + AMD
- Core (Pipeline) - 32 or 16bit
  - Makes the CPU act like multiple
- Hyperthreading (1 pipeline handing code)
- Register
- Speed (Clock)
- Overclocking for increasing the clock speed
- Hertz = Million times per second

## Hard Drive

- **Hard drives - Block Based** 
  - Can read/write data at the "block" level.
  - Create volumes (that can be partitioned).
  - You can choose the type of file system e.g. FAT32 or NTFS.
  - You can install an operating system on a block-based system.
- **File Storage - Network Attached**
  - Attached via a Network Interface Card.
  - Already has a file system (with partitions).
  - Often a shared directory in a company.
  - Is then shared over a network.
  - Use a "mount point" on Linux.
- **Object storage**
  - User uploads via a Web Browser.
  - Typically no hierarchy and expansive storage.
  - Upload an object to a container on the internet (HTTP protocol).
  - Typically uploaded over a RESTful API.

## Questions

- What is a CPU Register?
- What/who defines the network mask?
- How does automatic IP allocation work?