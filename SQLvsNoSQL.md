SQL|NoSQL
---|-----
SQL databases are best fit for heavy duty transactional type applications, as it is more stable and promises the atomicity as well as integrity of the data. | While you can use NoSQL for transactions purpose, it is still not comparable and sable enough in high load and for complex transactional applications.
SQL databases are table based databases | NoSQL databases are document based, key-value pairs, graph databases or wide-column stores
SQL databases have predefined schema | NoSQL databases have dynamic schema for unstructured data
SQL databases are vertically scalable |  NoSQL databases are horizontally scalable
SQL databases uses SQL ( structured query language ) for defining and manipulating the data | In NoSQL database, queries are focused on collection of documents. Sometimes it is also called as UnQL (Unstructured Query Language). The syntax of using UnQL varies from database to database.
For complex queries: SQL databases are good fit for the complex query intensive environment | NoSQL databases are not good fit for complex queries. On a high-level, NoSQL don’t have standard interfaces to perform complex queries, and the queries themselves in NoSQL are not as powerful as SQL query language.
SQL databases are not best fit for hierarchical data storage. | But, NoSQL database fits better for the hierarchical data storage as it follows the key-value pair way of storing data similar to JSON data. NoSQL database are highly preferred for large data set (i.e for big data). Hbase is an example for this purpose.
SQL databases emphasizes on ACID properties ( Atomicity, Consistency, Isolation and Durability) | whereas the NoSQL database follows the Brewers CAP theorem ( Consistency, Availability and Partition tolerance )


Nosql:
1. No relations
2. High flexibility
3. Data repetition
4. no integrity check
5. Easy calability

Sql 
1. relations
2. limited flexibility(strong schemas)
3. no data repetition
4. integrity checks
5. harder scalable
