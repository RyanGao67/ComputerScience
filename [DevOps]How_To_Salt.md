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

* Message over this zeroMQ tube connection are serialized and compressed using a library called messagepack(like json) 

![](./img/salt1.png)


### Transports

* ZeroMQ is the default

* Raw TCP transport provided by tornado

* HTTPS via salt-api

### Default master configuration


```
vi /etc/salt/master
```

* In general you can run the salt master as a non-root user because the master is sending messages to minions, but if you need to use salt to configure programmatically set up the salt master machine, you're going to want to keep user as root

### Minion config 

![](./img/salt2.png)



```
vi /etc/salt/minion
```


### Configuration directories

* Most of the packages (centos or ubuntu)will create /etc/salt/master.d or /etc/salt/minion.d automatically

* what they do is it will read all the conf file in that directory and load them all up 

* it is nice way to store all of the defaults in the main salt configuration and then just store your overrides in a conf file

For example: 

```

vi /etc/salt/minion.d/local.conf
```


in this file put 

```
master:192.168.56.101
id:jerry
```


```
service salt-minion restart
```

```
systemctl start salt-master
```

```
systemctl start salt-minion
```



### sdb

Salt is made of many pluggable interfaces. sdb is one of those. 
* One of the nice things about sdb is sdb values can be encrypted and they can be pulled into the salt configuration files and those settings are referenced or are decrypted or are looked up at the time that they are referenced, Not when the daemon is loaded. 

for example:
in the prevous local.conf file put in this :
```
mysecretsetting: sdb://lookup/path/here
```




### Authentication

* it uses public and private RSA keypairs

* it is a SSH handshake algorithm on top of zeroMQ  

* Masters and Minions generate keys on startup and then they do the public key exchange same as SSH does

* in /etc/salt/pki  master.pem is the private key ,   master.pub key is the public key other folders is to store the public key from minions


* then first time minion connects to the master, the minion send the public key to the master and the master accept the public key

* so if you're troubleshooting something or if you need to swap out the key, you need to delete on both minion and master 


### Encryption

* salt uses AES standard

* Rotating AES key every 24 hours and after a minion key is deleted or rejected

Salt uses this to create an encrypted pub sub connection 

So when that publish goes out to minions, that publish is encrypted using this key

and in order to deauthenticate a minion if you delete a minion's key then after you delete that key, salt will then rotate that key. So all of the other minions will authenticate and pick up that new key so that they can decrypt those publishes

* salt key is the interface to accessing all of these key management facilities

* One thing that we have to do with our system: centos sets up a firewall so we have to poke a hole in that firewall for the master port. To turn off firewall: iptables -F


* to  debug, take a look at /var/log/salt/minion

* consult help:

```		
salt-key -h
```

* accept keys 

```
salt-key -A
```


### Targeting


```
sudo salt -L jerry test.ping
```

```
sudo salt '*' test.ping
```


```
salt -G os_family:RedHat test.ping
```

```
salt -C 'G@os_family:RedHat and stu*' test.ping
```


### Grains

* grains are static information concerned about each minion as the minion daemon starts up. 

* (on minion) python functions ask the local system and return a little dictionary about whatever it is they query and that is combined into a larger dictionary of information about that system

* they are generated when the minion starts and they are kept in memory on the menu. they are also shipped up to the master and the master keeps a cache of them so that master can perform some out-of-band checks

List:    
```
sudo salt-call --local grains.items
```

Add: 
```
salt-call --local grains.setval foo Foo
```



### Flow vs state

* Flow is something ephemeral (it is something you run and it changes the system right now and the change isn't really tracked or enforced)

* State is enforcing the state of a system over time

* broadly flow and state are represented and solved as execution modules and state modules (execution modules are python functions in python modules, all the python functions return json serializable data)


### salt loader 

salt has what's called a loader. the salt loader is responsible for loading all of these module types

So the salt loader goes through and decides which one of these modules are applicable to the underlying operating system. (the modules are loaded into memory based on that)


### salt is pluggable system

* the core is a tiny little thing that maintains a always-on encrypted high speed communication channel and then surrounding salt is just a ton of pluggable interfaces(each pluggable interfaces has a corresponding module type). 




### Common salt modules

* Example: common execution modules(pkg, user, service, status, test, cmd, grains);  Salt internals as execution modules(match, cp)



```
salt jerry sys.doc test.ping
```

This is calling out to jerry and grabbing that documentation from jerry

```
salt jerry sys.doc test | less

```

This will show all the documents for all of the functions in the test module


```
salt jerry sys.list_modules
```

List all the execution modules on jerry

```
salt jerry sys.list_functions | less 

```

List all the functions on jerry
```
salt jerry pkg.list_pkgs | less
```

this lists the packages currently installed on the minion 

Each one of the package modules queries the underlying operating system and whatever is the most efficient quickest way to get that data. (in case of a red hat system, it will call out to RPM, in case of windows it will consult windows registry)


```
sudo salt -L jerry,stuart pkg.list_pkgs --out=txt | grep wget | cut -c -20
```


```
salt stuart user.list_users
```

```
salt stuart user.getent root
```


```
salt stuart service.get_running
```


```
salt stuart status.uptime

salt -L stuart,jerry status.diskusage

salt -L stuart,jerry test.version // show salt version installed

salt -L stuart,jerry test.versions_report // show salt version and dependencies version

salt -L stuart,jerry cmd.run 'whoami'

salt -L stuart,jerry cmd.run 'ls /etc/salt'

salt -L stuart,jerry cmd.run 'cat /etc/salt/grains'

salt -L stuart,jerry cmd.run_all 'cat /etc/salt/grains'

salt -L stuart,jerry cmd.script salt://myscript.sh  //this command takes a salt file, it will transfer the script from master to each one of minions and then executes it locally and return the result
```



