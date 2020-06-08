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
