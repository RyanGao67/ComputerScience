### In this post, I will document how to copy a database
In this example, I will use postgres.
* Step 1: 
This step will dump a file to sourcedb.sql
```
pg_dump    --host=mrblzqg4jsdgwf.cgdzj2vbkde2.ca-central-1.rds.amazonaws.com    
--port=5432    
--username=postgres    
--password    
--dbname=platform  
-f sourcedb.sql 
```

* Step 2:
This step will populate the databse with the sourcedb.sql
```
psql    
--host=mrblzqg4jsdgwf.cgdzj2vbkde2.ca-central-1.rds.amazonaws.com    
--port=5432    
--username=postgres    
--password    
platform_test  <  sourcedb.sql 
```
