1. MAC address : First half is the manufacturer's organizationally unique identifier(OUI) . Second half is a unique number
2. arp is used to discover the mac address of another divice . In ethernet mac address is attached to a devices by vendors, I need to know your mac address to talk to your devices in ether net
3. A media access control address(MAC address) is a unique identifier assigned to a NIC (network interface controller) for use as a network address in communications within a network setment
4. The dynamic host configuration protocal (DHCP) is a network managemnt protocal used on internet protocol networks whereby a DHCP server dynamically assigns an IP address and other network configuration parameters to each device on a network so they can communicate with other IP networks
5. Bridge = learns MAC addresses in software
6. Switch = learns MAC addresses much more quickly by using hardware ASICs(application specific integarted circuits)
7. switches send messages in local network (LAN)
8. switches work on layer 2 (TCP/IP, OSI model)
9. routers allow us to go from our Ethernet LAN, local area network onto the internet(WAN)
10. Protocals and Layers  
![](./img/Computer_Network1.png)  
11. terminology and layers  
![](./img/Computer_Network2.png)  
12. IP address is an layer 3 logical address assigned by an administrator(unlike MAC address that is burnt into NIC by manufacturer) 
13. An IP address is used to uniquely identify a device on the network and is used by routers to determine where the device is  
14. The IP address can change within a subnet for example when using DHP or Dynamic Host configuration Protocol   
15. An IP address is used to uniquely identify a device on the network and is used by routers to determine where that device is. So a router routes traffic to a destination IP address based on a hirarchy of network and host    
16. Every device on internet has a unique IP address so there are millions of IP address out there and no two devices can have the same IP address for communication on the internet    
17. IP v4 is connectionless protocal(there are no sessions formed when traffic is transmitted, the transmitter simply sends data without notification to the receiver, no status information is sent back from the receiver to the transmitter)  
18. TCP (transmission control protocol) is connection orientated TCP will setup a session(three way hand shake : syn, ack syn, ack)  
19. IP is not like TCP, it is connectionless protocol and each packet is treated independently of other packets (routers will route the traffic via different paths based on options like load balancing)   
20. Routing protocal determines the best path from A to B (IP)  
21. Hirarchical addressing structure (network and host porion). Routers based the routing decision on the network portion of the address rather than on the host portion of the address   
22. There is no guarantee of packet delivery. Any packet could be misdirected. (depends on the higher layer protocols to gurantee that TCP)   
23. We have the network address portion also known as the network ID, this identifies a specific network. Routers maintain routing tables that contain network addresses. It's important to realize that routers build their routing tables based on the network address and not on the host address. so they do not route packets from 1 interface to another interface based on IP addresses. They do their routing based on network address so they will look at the destination IP address in a packet and match that to a network address in their routing table to determine how traffic is routed. So an IP address  consiste of the network portion as well as the host portion which is also called the host ID, this identifies specific end point on a network such as a server, a printer, a PC and iphone.   
24. Classful networks were used in the internet from 1981 unitil the introduction of classless in domain routing(CIDR) in 1993   
25. Class A,B,C unicast Traffic  
26. Class D - multicast  
27. Class E - reserved for future or experimental purposed   
28. IPV6 does not use address classes  
29. IPV4 address classes was replaced by CIDR  
30. ![](./img/Computer_Network3.png)  
31. ![](./img/Computer_Network4.png)  
32. ![](./img/Computer_Network5.png)   
33. ![](./img/Computer_Network6.png)  
34. ![](./img/Computer_Network7.png)  
35. ![](./img/Computer_Network8.png)  
36. ![](./img/Computer_Network9.png)   
37. ![](./img/Computer_Network10.png)  
38. ![](./img/Computer_Network11.png)     
39. ![](./img/Computer_Network12.png)   
40. ![](./img/Computer_Network13.png)   


