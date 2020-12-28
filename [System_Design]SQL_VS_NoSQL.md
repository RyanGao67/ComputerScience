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


* Atomicity: The atomicity acid property in SQL. It means either all the operations (insert, update, delete) inside a transaction take place or none. Or you can say, all the statements (insert, update, delete) inside a transaction are either completed or rolled back.
* Consistency: This SQL ACID property ensures database consistency. It means, whatever happens in the middle of the transaction, this property will never leave your database in a half-completed state. 
If the transaction completed successfully, then it will apply all the changes to the database.
If there is an error in a transaction, then all the changes that already made will be rolled back automatically. It means the database will restore to its state that it had before the transaction started.
If there is a system failure in the middle of the transaction, then also, all the changes made already will automatically rollback. 
* Isolation: Every transaction is individual, and One transaction can’t access the result of other transactions until the transaction completed. Or, you can’t perform the same operation using multiple transactions at the same time. We will explain this SQL acid property in a separate article.
* Durability: Once the transaction completed, then the changes it has made to the database will be permanent. Even if there is a system failure, or any abnormal changes also, this SQL acid property will safeguard the committed data.


They are somewhat related but there's a subtle difference.

Atomicity means that your transaction either happens or doesn't happen.

Consistency means that things like referential integrity are enforced.

Let's say you start a transaction to add two rows (a credit and debit which forms a single bank transaction). The atomicity of this has nothing to do with the consistency of the database. All it means it that either both rows or neither row will be added.

On the consistency front, let's say you have a foreign key constraint from orders to products. If you try to add an order that refers to a non-existent product, that's when consistency kicks in to prevent you from doing it.

Both are about maintaining the database in a workable state, hence their similarity. The former example will ensure the bank doesn't lose money (or steal it from you), the latter will ensure your application doesn't get surprised by orders for products you know nothing about.
