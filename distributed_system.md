### Why we want a distributed system?
* Performance and storage
* Single point of failure
* high latency 
* Security and privacy

### What is distributed system
* A distributed system is a system of several processes, running on different computers, communicating with each other through the network, and are sharing a state or are working together to achieve a common goal

### What is a process? 
* When we compile a application into executable jar file or class, it is stored in our file system just like other text image music files. 
* When we launch an application, the operating sytem create a instance of that application in memory(process)   
* The process is entirely seperated from other processes on the computer   (no matter it is a instance of the same application or different application)
* The processes can communicate using the networks,  the file system, or memory  
**This is not a distributed system, because all the processes share the same computer**  

### What is node?
A process running on a dedicated machine as part of a distributed system (graph theory)  
### Cluster
* Collection of computers/nodes connected to each other
* The nodes in a cluster are working on the same task, and typically are running the same code  

### ZooKeeper
* Zookeeper is a high performance coordination service designed specifically for distributed systems
* Attemp 1 - manually elect a leader node(distributeing the work and collecting the result)
* Automatically elect the lead node - if the leader become unavailable, the system will automatically elect a new leader, and when the old leader recovers, the it will be no more a leader, it is a normal node
* ZooKeeper typically runs ina cluster of an odd number of nodes, higher than 3
* With zookeeper, instead of talking to each other, the nodes are talking to zookeeper server
![./img/distributed1.png](./img/distributed1.png)

* Zookeeper's abstraction and data model (each element in this tree(virtual file system) is a znode)
  * Znode is hybrid between file and ditrectory(store like a file, have children like a directory)
![./img/distributed2.png](./img/distributed2.png)
  
* Persistent znode: stay between sessions(If one application disconnects from zookeeper and reconnects again, the persistent znode created by our application stays intact with all its children and data)
* Ephemeral znode is deleted as soon as the application that creates the znode disconnects from zookeeper

* Leader Election algorithm
  * Every node connects to zooleeper volunteers to become the leader
  * Each node submits its candidacy by adding a znode that represents itself under the election parent
  * Zookeeper maintains a global order, it can name each znode according to the order of addition
  * After each node creating its znode, it will query the current children of the election parent
  * Because the order zookeeper provides us, each node, when querying the election parent, is garanteed to see all the znodes created prior to its own znode
  

### Setup zookeeper
* download zookeeper
* extract zookeeper
* Create a logs folder in zookeeper home dir
* in conf rename ***sampel.cfg to zoo.cfg
* vim zoo.cfg change DataDir to logs folder
* ./zkpServer.sh start
* ./zkpServer.sh status
* ./zkpServer.sh stop
* When server started, run ./zkcli.sh (client)
```
help
ls /
create /parent "parent data"
create /parent/child "some child data"

```

### Zookeeper Threading Model
* Application's start code in the main method is executed on the main thread
* When zookeeper object is created, two additional threads are craeted by zookeeper library
  * Event Thread(handles all the zookeeper client state chage event, eg, connection and disconnection with the zookeeper server, also handles custom znode watchers and triggers we subscribe to, Events are executed on Event thread inorder)
  * IO thread (handles all the network communication with zookeeper servers, handles zookeeper requests and responses responds to pings session menagement, session timeouts)
  
### Watcher and trigger
* We  can register a watcher when we call the methods 
  * getChildren()
  * getData()
  * exists()
 
* The watcher allows us to get a notification when a change happends 
* If ```getChildren(.., watcher)``` It will return a list of children of a given znode, we'll get notified when the list of children changed
* exits(znodePath, watcher) - get notified if a znode gets deleted or created
* getData(znodePath, watcher) - Get notified if a znode's data gets modified
* public ZooKeeper(String connectString, int sessionTimeout, Watcher watcher) - also takes a watcher
* Watchers registered with getChildren(), exists() and getData() are one-time triggers
* If we want to get future notifications, we need to register the watcher again

### A herd Effect
* A large number of nodes waiting for an event. 
* When the event happends all nodes get notified and they all wake up
* only one node can succeed
* indicates bad design, can negatively impact the performance and can completetly freeze the cluster
 
* Code Example Leader Election
[https://github.com/RyanGao67/Distributed_system_Leader_election/blob/master/src/main/java/testElectLeader/LeaderElection.java](https://github.com/RyanGao67/Distributed_system_Leader_election/blob/master/src/main/java/testElectLeader/LeaderElection.java)

* Code Example Leader ReElection
[https://github.com/RyanGao67/Distributed_system_leader_reElection/blob/master/leader-reelection-test/src/main/java/LeaderElection.java](https://github.com/RyanGao67/Distributed_system_leader_reElection/blob/master/leader-reelection-test/src/main/java/LeaderElection.java)

### Multithreading vs distributed systems
* For multithreading, passing a message from one thread to another is easy, since all threads running on the same application, they all had a shared memory space. race condition(lock), semaphore for condition variable for signalling
* For distributed system, we do not have shared memory any more (can only use network)

**Application HTTP, FTP, SMTP**
**Transport TCP, UDP**
**Internet IP, ICMP**
***DataLink Ethernet, 802.11, ARP, RAPR*

### Datalink
* Physical delivery of data over a single link
* in charge of encapsulation of the data
* Flow control
* Error detection
* Error correction
* Ethernet protocol
* MAC address 1 <=> MAX address2 <=> MAC address3
* postal trucks plans schedule ...
### Internet layer
* takes service from datalink layer
* delivering data across multiple network
* routing the packegs from source computer to destination computer (IP)
* In this layer obtaining the IP address of computer we want to communicate
* IP address the address of recepient 
(Using internet layer only, we can delever the package to target computer, but we do not know which application process is the package intended for)
### Transport layer
* end to end   
* one process to another process   
* each end point (socket) identify itself as a 16 bits port(8081)  the listening port is chosen ahead of time by the destination application, The source port is generated on the fly by the sender depending on the ports available at the moment   
* User datagram protocol(udp)  
  * Connectionless
  * best effort - unreliable
  * messages can be lost duplicatied redordered
  * based on a unit called datagram which is limited in size
  * UDP is preferred when the speed and simplicity is more important than reliability
  * UDP use cases : sending  debug information to a distributed logging service
  * really time video/ online game
  * allow broadcasting decoupling between sender and receivers
* transmission control protocol(tcp)
  * reliable
  * connection between 2 points 
  * need to be created before data is sent
  * shut down in the end
  * unlike individual datagram, tcp works like a streaming interface(more popular in distributed)
  
  * Because TCP is based on exactly two points , even if we have two sources connected to the same IP and port , data flow will be split into two socket, and will be handle seperately by the application and os
  * Each tcp connection is identified by the full tuple source IP port and destination ip port
  * The only problem is that TCP works as plain stream of bytes(not distinguishing which bytes belong to what message)

![./img/distributed3.png](./img/distributed3.png)  
### Application layer
* For the precious problem we need application layer
* Different protocal

![./img/distributed4.png](./img/distributed4.png)
![./img/distributed5.png](./img/distributed5.png) 
![./img/distributed6.png](./img/distributed6.png) 
  
  
  
### HTTP request structure
* method, Path, protocol version
* Http headers
* Message body
method : get head post put delete trace connect options patch


### load balancing
* Round robin
* source IP hash motivation. 
  * Example, maintaining online shopping cart state between connection loses or browser refresh (Requests from the same user should go to the same server throughout the entire session)
  * Sticky session Hash(Source.IP) = Server #3
  
* The problem of statistical load balancing (round robin)
  ** We assume that spreading the requests evently would also spread the load evenly
  ** Not all users are using the system the same way
  ** not all requests require the same amount of resources
  
* Least connection 
* Weighted Response Time
* Agent Based polity(Agent process will measure the CPU utilization, inbound outbound network traffic, disk operations memory utilization)

### Layer4 (Transport) Load balancing
* The load balancer performs simple TCP packets forwarding between the client and the abckend servers
* This the most low everhead balancing mode, since the load balancer don't inspect the content of the TCP stream beyong the first few packets
![./img/distributed7.png](./img/distributed7.png)

### Layer 7 (Application) load balancing 
* Can make smarter routing decisions based on the HTTP header
* Load balancer inspects TCP packets and HTTP header
* Can route requests to different clusters of servers based on 
  * Request URL
  * Type of requested data
  * HTTP methos 
  * browser cookies
  
  * The Request relative URI (path) is part of the HTTP header. If we configure the load balancer in TCP mode, the HTTP header will not be considered
![./img/distributed8.png](./img/distributed8.png)

### High Availability Proxy (HAProxy)
* HAproxy - Reliable, High Performance TCP/HTTP load balancer

### Database vs File System
* A file system is a lower level, general purpose approach to storage of data of any format, structure or size
* Best for unstructured data, or data with no relationship to other data
* for example video files audio files, text files, meory logs

* A database is a higher level of abstraction that may or may not store data in the file system underneath
  * an application that provides additional capabilities query landuage/engine, caching and performance optimizations
  * provides restrictions on structure, relationship and format
  * guarantees acid transactions
    * Atomicity
    * Consistency
    * isolation
    * durability
  * database is easy to build and replace

* SQL vs noSQL
* What we want from a DB
  * Availability
  * Scalability
  * Fault Tolerance
### Problems of centralized database issures
* Single point of failure
  * Losing a database is a lot worse than losing a compute node
  * Temporary failure to operate the business
* Performance bottleneck
  * Parallelism is limited to the number of cores in a machine
  * Limited connections the OS and network card can support
  * minimum latency depends on the geographical location of the database instance and the user
  * limited to the memory a single machine can have
  

![./img/distributed9.png](./img/distributed9.png)

### Sharding based on Key
* Sharding is done based on the record's key
* The key determines in which shard to find an existing record and to add a new record with a new key
* advantage: 
  * with monotonically increasing keys and a good hash function 
  * we can achieve even data distribution
  
* disadvantage
  * keys with close values will likely not fall in the same shard
  * range based queries will span multiple shards
  
![./img/distributed10.png](./img/distributed10.png)
![./img/distributed11.png](./img/distributed11.png)

### range based sharding 
* In range based strategy we divide the keyspace into multiple contiguous ranges
* Records with nearby keys will more likely end up in the same shard(range based queries will be a lot more efficient)

### Dynamic cluster resizing 
* consistent hashing(understand)

### replication vs sharding
* replica 
  * high available (router failure)
  * Fault Tolerance
  * scalability / performance
  
![./img/distributed12.png](./img/distributed12.png)

### Replicated Database architectures
* master slave
  * all the write operation gose  to master
  * all the read operation goes to slave
* master master
---> Eventual consistency
* in this model, if no further updates are made, eventually all readers will have access to the newest data
* However twmporarily some readers may see stale data
* provides lower latency and higher availability
* good choice for systems that do not need to have the most up to data data across the board
* example : posts updates to social media profile
* analytics for product ratings and number of reviews

---> strict consistency
* In strict consitency, the writer will not get an acknowledgement until we can guarantee that all the readers will see the new data
* slows down operations and limits system's availability(if some replicas are temporatily not accessible)
* essential for systems that need to be consistent across all the services
* Examples: bank account 
* number of items in a store's inventory
* avaiblable booked seats ina theater

### Quorum consensus - Record version
* Record: key | Data | Version
* Every update to a record increments the version number
* old record: key1 | data1 | 1
* new record: key1 | data2 | 2

R: Minimum number of nodes a reader needs to read from 
W: Minimum number of nodes a writer needs to write to
N: Number of nodes in the database cluster

R+W > N cuaranteed strict consistency (understand why)


### MongoDB
* create read update delete
* document _id is immutatle and connot be changed after the document's creation

### Message brokers
* Why we need a different way to communication between services
  * One of the properties of direct communication is it is inherently synchronous
  * broadcasting event to many sevices
  * The more servers want to receive the event the more direct connections the publisher servers need to open
  * traffic peaks and valleys 
* Message Broker difinition
  * Intermediary software (middleware) that passes messages between senders and receivers
  * May provide addtional capabilities like 
    * Data transformation
    * validation
    * Queueing
    * Routing
  * Full decoupling between sanders and receivers
  
  * tradeoff
    * To avoid the message broker from being the singlue point of failure or a bottleneck
    * Message brokers need to be scalable and fault tolerant
    * message brokers are distributed systems by themselves which makes them harder to design and configure
    * the latency, when using a message broker, in most cases is higher than when using direct communication
    
  * Use case
    * Distributed queue - Message dilivery froma single producer to a single consumer
    * publish / subscribe - Publishing of a message froma fingle publisher to a group of subscribed consumers
    
  
### Apache Kafka
* Distributed streaming platform, for exchanging messages between different servers
* Can be described as a Message Broker on a high level
* internally kafka is a distributed system that may use multiple message broker to handle the messages. 

* There are many great message brokers 
  * Open Source : RabbitMQ, ActiveMQ etc...
  * Proprietary messaging systems offered by the cloud vendors
  * apache kafka is open source and provides:
    * distributed queuing
    * publish/ subcribe
    * beautifully designed distributed system for high scalability and fault tolerance
    
* Kafka scalability through partitioning
  * Kafka topic partitioning allows us to scale a topic horizontally
  * more partitions in a topic -> higher parallelism
