### What is identity
An identity is typically defined by combinations of attributes such as Firstname, Lastname, address  
Basically some combination of attributes of attributes so that data is meaningful for organization while maintaining the identities. 


### What is context-based authentication
Context-based authentication balances trust against risk by letting you implement simple policies that allow (or deny) access to web applications based on contextual information - such as user role, group membership, device usage, location (IP address) and geographical location.

Context-based authentication dynamically adapts to context changes to:  
* Restrict access to high-risk applications that contain sensitive data to known office locations or to specific IP addresses (in the case of remote users)  
* Limit access to applications to approved or trusted devices  
* Require users to authenticate using 2-factor authentication (2FA) to access certain applications  

### LDAP
* LDAP is a protocol which is basically used to access your active directory object. Or for user authentication and authorization. 
* LDAP is lightweighted because most of the action is read operation. 

### Federation
A federation is a group of computing or network providers agreeing upon standards of operation in a collective fashion.

### With identity federation, a single system called a trusted identity provider(IDP) governs the authentication of users, with apps delegated over the authentication process to the IDP each time a user attempts to access them. 
// adfs//okta//jumpcloud

### SSO 
This idea, called signle sign-on(SSO), allows a user to enter one username and password in order to access multiple applications. 
There are multiple solutions for implementing SSO:
Three most common web security protocols:
* SAML (federated identity)
* OAuth
* OpenID

### SAML -- security assertion markup language
SAML is an xml based open standard for exchanging authentication data between independent websites

It is also called identify federation or federated authentication
saml uses security tokens(containing assertions) to pass information about a principal(usually an end user) between a saml authority(IDP), and a saml consumer(SP service rovider)
1. user authentication (when the user is last time authenticated, what kind of authentication eg, kerberos, ldap) 
2. user attributes
3. user authorization

### Oauth  
OAuth 2 is an authorization framework that enables applications to obtain limited access to user accounts on an HTTP service, such as Facebook, Google and twitter

It works by delegating user authentication to the service that hosts the user account, and authorizing third-party application to access the user account. 
