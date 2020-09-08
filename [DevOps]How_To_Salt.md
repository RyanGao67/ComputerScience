### Why salt?
* Fast enough that it doesn't require cached data or a database

* Distributed work to the Minions

* Constant network connection

* Fast encryption everywhere

* it is generic communication bus ---- useful for remote execution, state enforcement, more

* it has deterministic execution order ---- deterministic execution order,     ----states always run in the same order  

### Network topology

* master/minion

* it uses zeroMQ connection between all of those systems (use pub/sub and rep/req)

* Message over this zeroMQ cube connection are serialized and compressed using a library called message bag(like json) 

![./img/salt1.png]()



