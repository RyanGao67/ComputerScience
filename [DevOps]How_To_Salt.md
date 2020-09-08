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

So when that public goes out to minions, that publish is encrypted using this key

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
