
### Download and Setup Java 8 JDK:

sudo apt install openjdk-8-jdk   
### Download & Extract the Kafka binaries from 
[https://kafka.apache.org/downloads](https://kafka.apache.org/downloads)

### Try Kafka commands using bin/kafka-topics.sh (for example)

### Edit PATH to include Kafka (in ~/.bashrc for example) 
PATH="$PATH:/your/path/to/your/kafka/bin"

### Edit Zookeeper & Kafka configs using a text editor

zookeeper.properties: dataDir=/your/path/to/data/zookeeper

server.properties: log.dirs=/your/path/to/data/kafka

### Start Zookeeper in one terminal window: 
zookeeper-server-start.sh config/zookeeper.properties

### Start Kafka in another terminal window: 
kafka-server-start.sh config/server.properties

Important: For the rest of the course, don't forget to add the extension .sh to commands being run

### kafka topic
```
kafka-topics --zookeeper 127.0.0.1:2181 --topic first_topic --create --partitions 3 --replication-factor 1
kafka-topics --zookeeper 127.0.0.1:2181 --list
kafka-topics --zookeeper 127.0.0.1:2181 --topic first_topic --describe
kafka-topics --zookeeper 127.0.0.1:2181 --topic second_topic --create --partitions 6 --replication-factor 1
kafka-topics --zookeeper 127.0.0.1:2181 --list
kafka-topics --zookeeper 127.0.0.1:2181 --topic second_topic --delete
```

```
kafka-console-producer --broker-list 127.0.0.1:9092 --topic first_topic
kafka-console-producer --broker-list 127.0.0.1:9092 --topic first_topic --producer-property acks=all
kafka-console-producer --broker-list 127.0.0.1:9092 --topic new_topic   // no such topic
kafka-topics --zookeeper 127.0.0.1:2181 --list
kafka-topics --zookeeper 127.0.0.1:2181 --topic new_topic --describe
```

```
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning
kafka-console-producer --broker-list 127.0.0.1:9092 --topic first_topic 
```

```
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-second-application --from-beginning
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-second-application 

```


```
kafka-consumer-groups --bootstrap-server localhost:9092 --list
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-second-application
kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group my-first-application
kafka-console-consumer --bootstrap-server localhost:9092 --topic first_topic --group my-first-application
```


```
kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --execute --topic first_topic

kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-first-application
```
