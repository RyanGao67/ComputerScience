### Apache NiFi-Key Concepts
* Process Group - It is a group of Nifi flows, which helps a user to manage and keep flows in hierarchical manner. 
* Flow - It is created connecting different processors to transfer and modify data if required from one data source or sources to another destination data sources. 
* Processor - A processor is a java module responsible for either fetching data from sourcing system or storing it in destination system. Other processors are also userd to add attributes or change content in flowfile.
* Flowfile - it is the basic usage of Nifi, which represents the single object of the data picked from source system in Nifi. Nifi processor makes changes to flowfile while it moves from the source processor to the destination. Different events like create, clone, receive, etc. are performed on flowfile by different processors in a flow.
* Event - Event represent the change in flowfile while traversing through a Nifi Flow. These events are tracked in data provenance. 
* Data provenance - It is a repository. It also has a UI, which enables users to check the information about a flowfile and helps in troubleshooting if any issues that arise during the processing of a flowfile.

![](https://www.tutorialspoint.com/apache_nifi/images/apache_web_server.jpg)

### Flowfile Repository
This repository stores the current state and attributes of every flowfile that goes through the data flows of apache NiFi. The default location of this repository is in the root directory of apache NiFi. The location of this repository can be changed by changing the property named "nifi.flowfile.repository.directory".

### Content Repository
This repository contains all the content present in all the flowfiles of NiFi. Its default directory is also in the root directory of NiFi and it can be changed using "org.apache.nifi.controller.repository.FileSystemRepository" property. This directory uses large space in disk so it is advisable to have enough space in the installation disk.

### Provenance Repository
The repository tracks and stores all the events of all the flowfiles that flow in NiFi. There are two provenance repositories - volatile provenance repository (in this repository all the provenance data get lost after restart) and persistent provenance repository. Its default directory is also in the root directory of NiFi and it can be changed using "org.apache.nifi.provenance.PersistentProvenanceRepository" and "org.apache.nifi.provenance.VolatileProvenanceRepositor" property for the respective repositories.

### What is dataflow
* Moving some content from A to B(eg. Log --> SQL --> big data)

### What is a flowfile?
Flowfile = Actual Data -> CSV, JSON, XML, Plaintext, SQL, Binary, etc
Flowfile - Abstrction of data in Nifi
A processor can generate new Flow file by processing an existing Flowfile  or ingesting new data from any resource. 


### What is a process group? 
* Set of processors can be combined in Nifi to form a process Group
* Process Group helps to maintain large and complex data flow
* input and Output ports are used to move data between Process Groups

### What is a controller service
* A controller service is a shared service that can be used by a processor
* Controller Service can be hold DB connection details
* We can create controoler service for csv Reader. Json writer etc

### Components of Flowfile
* A flowfile is composed two components 
* content 
* Attributes

### Construct Using nifi flowfiles
* A processor can either add, update or remove attributes of a Flowfile
* Or change content of Flowfile

### Update the attributes or content or both using various processors available in Nifi to design your Dataflow
* Flowfiles are persisted in the disk
* Flowfiles are passed-by-reference
* A new flowfile will be created if the content of the existing flowfile is modified or new data is ingested from source
* New Flowfile will not be created if the attributes of the existing Flowfile is modified.

### Apache Nifi use cases:
* What Apache Nifi is good at:
  * Reliable and secure transfer of data between systems
  * Delivery of data from sources of analytic platforms
  * Enrichment and preparation of data:
    * Conversion between formats
    * Extraction/Parsing
    * Routing decisiion

* What Apache Nifi shouldn't be used for:
  * Distributed Computation
  * Complex Event processing
  * Joins, rolling windows, aggregates opperations
  
### Concept: Processor
* Applies a set of transformations and rules to FlowFiles, to generate new FlowFiles
* Any processor can process any FlowFile
* Processors are parsing Flowfile references to each other to advance the data processing
* They are all running in parallel(different threads)

### Connector
* It's basically a queue of all the Flowfiles that are yet to be processed by processor2
* Defines rules about how FlowFiles are prioritized (Which ones first, which ones not at all)
* Can define backpressure to avoid overflow in the system
