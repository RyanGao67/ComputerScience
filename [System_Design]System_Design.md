Some useful material: 

[https://www.1point3acres.com/bbs/thread-208829-1-1.html](https://www.1point3acres.com/bbs/thread-208829-1-1.html)

[https://www.evernote.com/shard/s576/client/snv?noteGuid=7e58b450-1abe-43a8-bf82-fbf07f1db13c&noteKey=049802174415b418a2e65f75b744ab72&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs576%2Fsh%2F7e58b450-1abe-43a8-bf82-fbf07f1db13c%2F049802174415b418a2e65f75b744ab72&title=Interview%2BPreparation](https://www.evernote.com/shard/s576/client/snv?noteGuid=7e58b450-1abe-43a8-bf82-fbf07f1db13c&noteKey=049802174415b418a2e65f75b744ab72&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs576%2Fsh%2F7e58b450-1abe-43a8-bf82-fbf07f1db13c%2F049802174415b418a2e65f75b744ab72&title=Interview%2BPreparation)

[https://github.com/checkcheckzz/system-design-interview](https://github.com/checkcheckzz/system-design-interview)

[https://www.interviewbit.com/practice/](https://www.interviewbit.com/practice/)

### Fan in vs Fan out
[https://it.toolbox.com/blogs/craigborysowich/design-principles-fan-in-vs-fan-out-050407](https://it.toolbox.com/blogs/craigborysowich/design-principles-fan-in-vs-fan-out-050407)   

### Scalability
* Scalability is the capability of a system to manage increased demand
* Good examples of horizontal scaling are Cassandra and MongoDB as they both provide an easy way to scale horizontally by adding more machines to meet growing needs. Similarly, a good example of vertical scaling is MySQL as it allows for an easy way to scale vertically by switching from smaller to bigger machines. However, this process often involves downtime.


### Reliability
* In simple terms, a distributed system is considered reliable if it keeps delivering its services even when one or several of its software or hardware components fail.
==>replica


### Availability
* It is a simple measure of the percentage of time that a system, service, or a machine remains operational under normal conditions.

### Reliability Vs. Availability
If a system is reliable, it is available. However, if it is available, it is not necessarily reliable.  
eg. Let’s take the example of an online retail store that has 99.99% availability for the first two years after its launch. However, the system was launched without any information security testing. The customers are happy with the system, but they don’t realize that it isn’t very reliable as it is vulnerable to likely risks. In the third year, the system experiences a series of information security incidents that suddenly result in extremely low availability for extended periods of time. This results in reputational and financial damage to the customers.   


### Efficiency
* To understand how to measure the efficiency of a distributed system, let’s assume we have an operation that runs in a distributed manner and delivers a set of items as result. 
* Two standard measures of its efficiency are :
  * the response time (or latency) that denotes the delay to obtain the first item
  * the throughput (or bandwidth) which denotes the number of items delivered in a given time unit (e.g., a second). 
  
  
### Serviceability or manageability
* Serviceability or manageability is the simplicity and speed with which a system can be repaired or maintained; 


### Load Balancer
* It helps to spread the traffic across a cluster of servers to improve responsiveness and availability of applications, websites or databases. LB also keeps track of the status of all the resources while distributing requests. If a server is not available to take new requests or is not responding or has elevated error rate, LB will stop sending traffic to such a server.
