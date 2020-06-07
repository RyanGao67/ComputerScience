### Serverless
1. No servers to provision or manage
2. Scales with usage
3. Never pay for idle
4. Availability and fault tolerance are built in 

### Datalake vs Data warehouse
Datalake  | Data warehouse
----------- | -----------
Semi-structured, unstructured, structured data | structured data
schema on read | schema on write
Data science, predictive analysis, BI | SQL based BI use cases
Great for storing granular data, raw as well as prcessed data | Great for storing grequently accessed data as well as data aggregates and summary
seperation of compute and storage | Tightly coupled compute and storage

 
### WHY Data lake 
Description: 
* Collect and store any data at any scale and at low cost
* Secure the data and prevent unauthorized access
* Catalogue, search and discover data 
* Decouple compute from storage 
* Future Proof against a highly changing complex ecosystem

Reasons:
* Exponential growth in data
* Diversified consumers
* Multiple access mechanisms

### Redshift 
Redshift is cluster based, not serverless, eg. one-node cluster

### Glue
AWS glue data catalog - The persistent metadata

### OLTP
* RDBMS 
* Fast insert/update
* ad-hoc queries
* heavily concurrent
* heavily normalized via 3NF
* row-oriented
* not meant for heavy volumn(<TB)

### OLAP
* Dataware house
* Analytics queries on huge datasets
* Query speed not priority 
* Few internal users
* Aggregated data
* Column oriented

### AWS Glue terminology
1. AWS Glue data catalog 
   * The persistent metadata store in AWS glue. Each AWS account has one AWS Glue Data catalog. It contains table definition, job definition and other control information to manage your aws glue environment. 
   * AWS glue data catalog is a managed service that lets you store, annotate and share metadata in AWS. It provides a uniform repo where disparate systems can store and find metadata to keep track of data in data silos
   * You can use AWS IAM policies to control access to the data sources managed by the AWS glue data catalog
   * Data catalog also provides comprehensive audit and governance capabilities, with schema change tracking, lineage of data
   * Table  ---------  A table in AWS glue data catalog consists of the names of columns, data type definitions, and other metadata about a base dataset. The schema of your data is represented in your AWS glue table definition. The actual data remains in its original data store. 
   * Classifier ---- Determines the schema of your data
   * Crawler ---- A program that connects to a data store (source or target), progresses through a prioritized list of classifiers to determine the schema for your data, and creates metadata in the AWS glue data catalogue. 
   * Development endpoint - An environment that you can use to develop and test your aws glue script
   
### glue network
* To run your ETL jobs, AWS Glue must be able to access your data store. If a job doesn't need to run in your virtual provate cloud (VPC) subnet, no additional configuration is needed. For example, transforming data from amazon s3 to amazon s3.   
* If a job needs to run in your VPC subnet, AWS glue sets up elastic network interfaces that enable your jobs to connect securely to other resources within your VPC. Each elastic network interface is assigned a private IP address from the IP address range within the subnet you specified. (No public IP addresses are assigned. The specified security groups are applied on the elastic network interface)  
* All JDBC data stores that are accessed by the job must be available from the VPC subnet.   
* To access Amazon s3 from within your VPC, a VPC endpoint is required.   
* If your job needs to access both VPC resources and the public internet, the VPC needs to have a Network Address Translation(NAT) gateway inside the VPC.  
* A job or development endpoint can only access one VPC(and subnet) at a time. If you neet to access data stores in different VPCs, you have the following options:
  * Use VPC peering to access the data stores. 
  * Use an amazon s3 bucket as an intermediary storage location. Split the work into two jobs, with the amazon s3 output of job 1 as the input of job 2
  
