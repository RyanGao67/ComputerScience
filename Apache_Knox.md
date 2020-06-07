### Before everything else first implementing this server will be helpful to understand other stuff. 
[https://github.com/RyanGao67/JWTtest20200603/blob/master/app.js](https://github.com/RyanGao67/JWTtest20200603/blob/master/app.js)

### How do API Gateways and JWTs fit together? 
* As a request comes in, one of the first jobs a system will do is decide whether the request should be serviced.   
* It is inefficient to have every microservice take on the overhead of authenticating the user, and then loading the user details from the user information microservice. 
* Instead, we can have the API Gateway authenticate the request, therefore removing the need for what would likely be duplicated authentication effort within each service, and the API Gateway can pass on an identifier that the upstream microservices can use.    
* If we take a look at the Kong Api Gateway, with the aid of the official Kong JWT authentication plugin, it does exactly this. It issues and verifies JWTs, and after successful authentication it will append either an X-Consumer-Username or X-Consumer-Custom-ID parameter to the upstream service.    
* At this point, we’ve demonstrated how JWTs can be used for authentication by an API gateway, but the usage of a JWT at this point is not providing significant benefits as opposed to using some other form of token.    
* Individual microservices will still face the challenge of needing to load the user information, probably by using a user information microservice.
* Luckily, this is where the payload section of JWTs comes to the rescue. The API Gateway can manage authentication whichever way it likes, for example, exposing an auth endpoint which accepts a Username/Password combination and returning a token of some sort for future requests. 
* The token could be a JWT, or it could be some other form of token. However, upon verifying this ‘public facing token’, the API Gateway can then load the user details from the user information microservice, and generate a JWT containing this user information in the payload section of this new JWT, which will then be passed on the requests it makes to the microservices. 
* Once a microservice receives a request, it can decode and verify signature of the JWT and obtain the user information it needs from the JWT payload without the need to contact the user information service. Additionally, each microservice can then include this JWT to any other microservices which they call themselves, all the while token sent out into the public world by the API Gateway is completely separate and can contain the minimum information required.

### Introduction
* Provide authentication and token verification at the perimeter
* Enable authentication integration with enterprise and cloud identify management system
* Provide service level authorization at the perimeter
* Limit the network endpoints(and therefor firewall holes) required to access a Hadoop cluster
* Hide the internal Hadoop cluster topology from potential attackers

### Knox folder {GATEWAY_HOME}
Some important folder
```
conf/	Contains configuration files that apply to the gateway globally (i.e. not cluster specific ).
data/	Contains security and topology specific artifacts that require read/write access at runtime
conf/topologies/	Contains topology files that represent Hadoop clusters which the gateway uses to deploy cluster proxies
```

### Default Topology URLs
* The knox gateway has provided a feature called the default Topology. 
* This refers to a topology deployment that will be able to route URLs without the additional context( that the gateway uses for differentiating from one hadoop cluster to another)
* This allows the URLs to match those used by existing clients (that may access WEBhdfs through the hdfs abstraction)

* The configuration for the default topology name is found in gateway-site.xml as a property called: default.app.topology.name.  

* The default value for this property is empty.

* When deploying the sandbox.xml topology and setting default.app.topology.name to sandbox, both of the following example URLs work for the same underlying Hadoop cluster:

https://{gateway-host}:{gateway-port}/webhdfs
https://{gateway-host}:{gateway-port}/{gateway-path}/{cluster-name}/webhdfs
* These default topology URLs exist for all of the services in the topology.

### Fully qualified URLs 
[https://knox.apache.org/books/knox-1-4-0/user-guide.html#Fully+Qualified+URLs](https://knox.apache.org/books/knox-1-4-0/user-guide.html#Fully+Qualified+URLs)
