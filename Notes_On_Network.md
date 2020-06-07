Ethernet protocols: physical protocols, describes the medium(wiring), the connections(RJ-45 port), and the signal(voltage lovel on a wire)
TCP/IP protocols: logical protocols, software controlling how and when data is sent and received to computers, supporting physical protocals

Example common TCP/IP suite of Protocols:  
web communication: HTTP  
E-mail: POP3, SMTP, IMAP  
File Transfers: FTP  

### OSI models
what is it？
* The open system interconnection reference model  
* A **conceptual** framework showing us how data moves throughout a network

Why it is developed?
* Created to give us a guide to understand how network operates
* it is only a reference model, so don't get wrapped up in the details
* it is not implemented in the real world, TCP/IP is. 

The OSI Model Stack breaks down the complex task of computer-to-computer network communications into 7 layers
* upper layers (host layers) handled by the host computer and performs application-specific functions, such as data formatting, encryption, and connection management
* Lower Layers (media layers) provide netword-specific functions, such as routing, addressing, and flow control

* upper layers:   
data - application layer,                                       => Application Layer  
data - presentation layer(data representation & encryption)     => Application Layer  
data - Session Layer(Interhost communication),                  => Application Layer  
* lower layers: 
segment - Transport Layer                                             => Transport layer  
Packet  - Network Layer  (Path determination & IP logical addressing  => Internet Layer  
Frame   - Data link layer (MAC and LLC (Pyhsical Addressing)          => network interface layer  
Bit     - Physical Layer (Media, signal and binary transmissions)     => network interface layer  


### TCP/IP Model
The TCP/IP is based on a 4 layer model that is similar to the OSI model
* Application layer : FTP, NFS, SNMP, SMTP, HTTP, POP3
* Transport layer : TCP, UDP
* internet layer: IP, ARP
* Network interface layer: Ethernet, Token Ring


### MAC address
* physical address of the network adapter card(burt into rom chip)
* OSI layer 2 (Data link) layer address
* TCP/IP layer 1( network interface ) layer address

00:21:70:6f:06:f2 first three bytes are assigned by the IEEE to the manufacturer, the last three bytes are usually assigned sequentially

IP addresses
* Network (OSI layer 3) address
* Logical Address
* Allows network-to-network communication via routers(Distant WAN communication)
* Dotted decimal notation 192.168.100.25

MAC Addresses
* Data link (OSI layer 2) addresses
* physical addresses 
* physically burned on NIC
* Allows internetwork communication via hubs, switches, and routes (Local LAN communication)

### Half versus Full duplex communication
* Half duplex can send and receive data, but not at the same time.
* can send and receive data simultaneously

### Ethernet
* refer to a family of standards that together define the physical and data link layers of the worlds's most popular type of LAN
* Is a standard communications protocol for building a local area network(LAN) speeds, cabling, connectors, equipment
* Modern Ethernet uses twisted pair of fiber cable
* Is is a standard model for LANs worldwide. 
