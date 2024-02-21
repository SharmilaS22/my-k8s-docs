# Open Systems Interconnection 

framework for network transmissions

7 layers

## 1. **Physical Layer (Layer 1):**
**Function:** 
   - physical connection between devices
   - the transmission of raw data bits over a physical medium. 
   - It defines specifications such as voltage levels, cable types, and data transmission rates.

**Examples:** Ethernet cables, Wi-Fi signals, fiber optic cables, and the hardware components (like network interface cards) that transmit and receive data signals.

## 2. **Data Link Layer (Layer 2):**
**Function:** 
- reliable
- error-free 
- transmission of data frames between _adjacent nodes_ on a network.
- in correct order.
   
- **Sublayers:** 
     - **Logical Link Control (LLC):** Handles error checking and flow control.
     - **Media Access Control (MAC):** Controls access to the physical network medium, such as Ethernet MAC addresses.

**Examples:** Ethernet frames, MAC addresses, and protocols like Ethernet, Wi-Fi (802.11), and Point-to-Point Protocol (PPP).

## 3. **Network Layer (Layer 3):**
**Function:**
- focuses on the _logical addressing and routing_ of data packets between different networks.
- determines the best path for data - considering factors like network topology and congestion.

**Examples:** IP addresses, routers, and protocols like Internet Protocol (IP), Internet Control Message Protocol (ICMP), and Routing Information Protocol (RIP) or Open Shortest Path First (OSPF).

## 4. **Transport Layer (Layer 4):**
**Function:** 
- reliable end-to-end communication between hosts. 
- It breaks data from the upper layers into smaller segments, - adds sequence numbers for reassembly
-  provides error detection and correction.
**Protocols:** 
- Transmission Control Protocol (TCP) (reliable, connection-oriented), 
- User Datagram Protocol (UDP) (faster, less reliable communication), good for streaming.

## 5. **Session Layer (Layer 5):**
**Function:** 
- establishes, manages, and terminates _sessions_ (or connections) between applications.
- syncs data streams, allowing data exchange between devices.

**Examples:** Remote Procedure Call (RPC), NetBIOS, and session management protocols like Session Initiation Protocol (SIP).

## 6. **Presentation Layer (Layer 6):**
**Function:** 
- handles data representation and translation (ensuring that information sent from one system can be properly _understood_ by another). 
- data formatting, encryption, and compression.

**Examples:** ASCII, JPEG, MPEG, and encryption protocols like _Secure Sockets Layer_ (SSL) and _Transport Layer Security_ (TLS).

## 7. **Application Layer (Layer 7):**
**Function:** 
- provides network services directly to end-users or applications.
- includes protocols and interfaces used by apps.

**Examples:** Hypertext Transfer Protocol (HTTP), File Transfer Protocol (FTP), Simple Mail Transfer Protocol (SMTP), and Domain Name System (DNS).
