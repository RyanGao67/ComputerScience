### What is sharding?
* Sharding is a database architecture pattern related to horizontal partitioning — 
* the practice of separating one table’s rows into multiple different tables, known as partitions. 
* Each partition has the same schema and columns, but also entirely different rows. 
![https://github.com/RyanGao67/ComputerScience/blob/master/img/db1.png](https://github.com/RyanGao67/ComputerScience/blob/master/img/db1.png)

* Sharding involves breaking up one's data into two or more smaller chunks, called logical shards
* The logical shards are distributed across sepatate database nodes, which is physical shards(hold multiple logical shards)


Database shards exemplify a **shared-nothing** architecture. This means that the shards are autonomous; 
they don’t share any of the same data or computing resources. 
In some cases, though, it may make sense to replicate certain tables into each shard to serve as reference tables.
For example, let’s say there’s a database for an application that depends on fixed conversion rates for weight measurements. By replicating a table containing the necessary conversion rate data into each shard, it would help to ensure that all of the data required for queries is held in every shard.
       
         
Oftentimes, sharding is implemented at the application level, meaning that the application includes code that defines which shard to transmit reads and writes to. However, some database management systems have sharding capabilities built in, allowing you to implement sharding directly at the database level.

### Benefit of Sharding    
* horizontal scaling, also known as scaling out.     
* Another reason why some might choose a sharded database architecture is to speed up query response times.    
* Sharding can also help to make an application more reliable by mitigating the impact of outages.    

### Drawbacks  
* Rather than accessing and managing one’s data from a single entry point, users must manage data across multiple shard locations, which could potentially be disruptive to some teams.
* One problem that users sometimes encounter after having sharded a database is that the shards eventually become unbalanced. By way of example, let’s say you have a database with two separate shards, one for customers whose last names begin with letters A through M and another for those whose names begin with the letters N through Z. However, your application serves an inordinate amount of people whose last names start with the letter G. Accordingly, the A-M shard gradually accrues more data than the N-Z one, causing the application to slow down and stall out for a significant portion of your users. The A-M shard has become what is known as a database hotspot. In this case, any benefits of sharding the database are canceled out by the slowdowns and crashes. The database would likely need to be repaired and resharded to allow for a more even data distribution.
* A final disadvantage to consider is that sharding isn’t natively supported by every database engine. For instance, PostgreSQL does not include automatic sharding as a feature, although it is possible to manually shard a PostgreSQL database. There are a number of Postgres forks that do include automatic sharding, but these often trail behind the latest PostgreSQL release and lack certain other features. 

### Key Based Sharding    
![https://github.com/RyanGao67/ComputerScience/blob/master/img/db2.png](https://github.com/RyanGao67/ComputerScience/blob/master/img/db2.png)   
Key based sharding, also known as hash based sharding, involves using a value taken from newly written data — such as a customer’s ID number, a client application’s IP address, a ZIP code, etc. — and plugging it into a hash function to determine which shard the data should go to. 
### Range Based Sharding   
![https://github.com/RyanGao67/ComputerScience/blob/master/img/db3.png](https://github.com/RyanGao67/ComputerScience/blob/master/img/db3.png)  
### Directory Based Sharding   
![https://github.com/RyanGao67/ComputerScience/blob/master/img/db4.png](https://github.com/RyanGao67/ComputerScience/blob/master/img/db4.png)

Before sharding, you should exhaust all other options for optimizing your database. Some optimizations you might want to consider include:

* Setting up a remote database. If you’re working with a monolithic application in which all of its components reside on the same server, you can improve your database’s performance by moving it over to its own machine. This doesn’t add as much complexity as sharding since the database’s tables remain intact. However, it still allows you to vertically scale your database apart from the rest of your infrastructure.
* Implementing caching. If your application’s read performance is what’s causing you trouble, caching is one strategy that can help to improve it. Caching involves temporarily storing data that has already been requested in memory, allowing you to access it much more quickly later on.
* Creating one or more read replicas. Another strategy that can help to improve read performance, this involves copying the data from one database server (the primary server) over to one or more secondary servers. Following this, every new write goes to the primary before being copied over to the secondaries, while reads are made exclusively to the secondary servers. Distributing reads and writes like this keeps any one machine from taking on too much of the load, helping to prevent slowdowns and crashes. Note that creating read replicas involves more computing resources and thus costs more money, which could be a significant constraint for some.
* Upgrading to a larger server. In most cases, scaling up one’s database server to a machine with more resources requires less effort than sharding. As with creating read replicas, an upgraded server with more resources will likely cost more money. Accordingly, you should only go through with resizing if it truly ends up being your best option.
