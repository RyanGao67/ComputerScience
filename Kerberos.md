### What is kerberos 
* Kerberos is an authentication protocol that can be used for single sign on.
* Kerberos is an authentication protocol that for trusted hosts on untrusted networks   
  * Kerberos does not provide any guarantees if the computers being used oare vulnerable
  * KDC includes three parts (KDC kerberos distribution center, client, server)

To Demonstrate trust we are going to need to use some secrets. There is only one secret, NTLM Hash. Kerberos uses shared secrets for authentication in a windows domain there is only one, the NTML Hash. The password hash is used to encropt everything in MS Kerberos.     
![](/img/kerbo.jpg) 


Ticket
An authorisation to do something.
Ticket Granting Ticket (TGT)
An initial Ticket that confirms to the Realm that you have provided adequate proof of who you claim to be. The actual ticket name is typically
Realm
(Designated R below.) An administrative grouping for network services, that typically (but not always) coincides with a network domain or sub-domain (eg dfusion.com.au). Realms are often named the same as the domain but in UPPER CASE (eg DFUSION.COM.AU) (mostly for sanity in troubleshooting AFAIK, although som software seems to assume/enforce upper case).
Authentication Service (AS)
The service that confirms a user's identity.
Ticket Granting Service (TGS)
The service that hands out the TGT once authentication is complete.




### About Kerberos Principals and keys
* The simplest, initial, answer can be that a principal is the analogous of a user name in a multiuser OS. So why do we call it principal ? And why do you hear variations like 'User Principal' or 'Service Principal' ?  
* The reason why the term principal is used is because 'user' is indeed insufficient, too generic and misleading. In Kerberos there are many actors that need keys, any actor that need a key needs to be represented by an identifier. These identifiers are compounded strings called 'principals'.  

* Anatomy of a principal  
A principal is a set of components represented by strings. One very important component is the realm name, each principal is always fully qualified with the name of the realm, The realm is represented by the last component in the string form. It is placed after an @ sign and is conventionally all upper case. The first part of the principal, instead, represents a specific identity within the realm. The first part can be split in multiple components joined by a / character.

Example:

component1 / component2 @ REALM
The simplest principals are actually what we think of users, generally actual people. The simplest identifier to represent users uses just one component and the realm. For example, the principal simo@EXAMPLE.COM represents a user named 'simo' that belongs to a realm named EXAMPLE.COM

The component is what we think of a user name, pretty simple so far. The realm as you can see resembles a domain name. That is on purpose as normally Kerberos realms are tied to DNS domain names, although not strictly required by the protocol specifications. Some implementations of Kerberos like Active Directory makes this a requirement. In AD the realm name is always the (DNS) domain name.

* Another set of extremely important principals are the so called Service Principals. These principals represent actual programs or computers. Their form normally comprises two components, a service part and a fully qualified hostname.

Example:

nfs/server.example.com@EXAMPLE.COM
Let's analyze this principal name. The first component represents the service being used, in this case 'nfs' is used to represent a NFS server. Other well know service types are 'HTTP', 'DNS', 'host', 'cifs', etc... The second component is a DNS name. This is the server's own name. The realm specifies that this service is bound to the EXAMPLE.COM realm.

Why this specific convention was chosen to represent a specific NFS Server ?

The reason is that the Kerberos protocol does not offer a name resolution service. So a convention was devised to make it easy for a client to automatically compute what is the principal name of a target service they want to contact based on 2 easy to know names: the service type, and the name of the host that is offering it. This is necessary because this name is used by the client to contact the KDC and ask for a ticket for that specific service. If the client doesn't know the specific name of the target service, it cannot ask for a ticket.

The service type is easy to know, an NFS client is used to connect to an NFS server and the type can simply be hard coded to 'nfs', or can be set into a configuration file quite easily, it will be the same for all services of that type across the network.

The host name is also generally a well known name. When a user wants to connect to a specific server it has to identify it somehow to the NFS client, and that usually means giving the mount utility a server 'name'. Same for HTTP, you have to give the browser a server name to contact as part of a URL and so on. The only limitation, in case of Kerberos is that you need the canonical form upfront (although there is work to relax this requirement). DNS is often used to find out the canonical form from a shorter name or sometimes even an IP address (but see this post about reverse resolution).



**The question at this point is why do we need principals to represent services or whole hosts ? The answer is that each service you want to contact needs keys in order to decrypt the tickets you present them to authenticate yourself.**

[https://ssimo.org/blog/id_016.html#:~:text=Each%20principal%20is%20associated%20to,called%20a%20shared%20key%20system.](https://ssimo.org/blog/id_016.html#:~:text=Each%20principal%20is%20associated%20to,called%20a%20shared%20key%20system.)
