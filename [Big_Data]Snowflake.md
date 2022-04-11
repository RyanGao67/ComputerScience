### create dataware houses
```
USE ROLE SYSADMIN;

DROP warehouse TEST_WH;

CREATE WAREHOUSE TEST_WH WITH WAREHOUSE_SIZE = 'XSMALL'
WAREHOUSE_TYPE = 'STANDARD' AUTO_SUSPEND = 600 -- automatically suspend the virtual warehouse after 10 minutes of not being used
AUTO_RESUME = TRUE
MIN_CLUSTER_COUNT = 1
MAX_CLUSTER_COUNT = 3
SCALING_POLICY = 'STANDARD'
COMMENT = 'This is our first test warehouse';


```

What is a data warehouse?    
So the purpose of a data warehouse is to consolidate and integrate different data sources and use them for reporting and data analysis.


### Multi-Clustering

when we have more queries, we need to queue the queries, and wait. To reduce waiting time, we can have multi-clusters, more warehouses can process the query. 

If the query is more and more complex, we need bigger warehouse, like M or L or XL, etc

Scaling policy: 

Standard: minimizes queuing by favoring starting additional clusters over conserving credits     
Economy: conserves credits by favoring keeping running clusters fully-loaded rather than starting additional clusters

### create database and tables
```
CREATE DATABASE OUR_FIRST_DB;

CREATE TABLE "OUR_FIRST_DB"."PUBLIC"."SECOND_TABLE" (
 "AGE_CUSTOMER" INTEGER NOT NULL,
 "CUSTOMERNAME" STRING NOT NULL
)
```

### Load data

```
//Creating the table / Meta data

CREATE TABLE "OUR_FIRST_DB"."PUBLIC"."LOAN_PAYMENT" (
  "Loan_ID" STRING,
  "loan_status" STRING,
  "Principal" STRING,
  "terms" STRING,
  "effective_date" STRING,
  "due_date" STRING,
  "paid_off_time" STRING,
  "past_due_days" STRING,
  "age" STRING,
  "education" STRING,
  "Gender" STRING);
  
  
 //Check that table is empy
 USE DATABASE OUR_FIRST_DB;

 SELECT * FROM LOAN_PAYMENT;

 
 //Loading the data from S3 bucket
  
 COPY INTO LOAN_PAYMENT
    FROM s3://bucketsnowflakes3/Loan_payments_data.csv
    file_format = (type = csv 
                   field_delimiter = ',' 
                   skip_header=1);
    

//Validate
 SELECT * FROM LOAN_PAYMENT;
```

### Snowflake Roles
AccountAdmin <-- SecurityAdmin <-- securityAdmin <-- useradmin <-- public
             <-- SysAdmin <-- custom role1 

AccountAdmin can do anything SecurityAdmin and Sysadmin can do. etc

### Loading Data
1. bulk loading: most frequent method, users warehouses, loading from stages, copy command, transformations possible
2. continuous loading: designed to load small volumes of data, automatically once they are added to stages, lates results for analysis, snowpipe

### stages
* not to be confused with data warehouse stages / staging
* location of data files where data can be loaded from 

External stage: s3, GCP, Microsoft Azure

And it is important to know that these external stages are just a database object that are created in a database and to be more precise, in a schema that is in a database.  And we can create those stages using the create stage command.

And these database objects have certain properties such as the you are such as some access settings and maybe some more settings that are important in regards to the connection to this location.

But also, I want to mention here that there can be additional costs if this external stage is in a region or is a different platform, then we have selected in our snowflake account.

Internal stage: local storage maintained by snowflake


### Create a stage & load data
```
// Database to manage stage objects, fileformats etc.

CREATE OR REPLACE DATABASE MANAGE_DB;

CREATE OR REPLACE SCHEMA external_stages;


// Creating external stage

CREATE OR REPLACE STAGE MANAGE_DB.external_stages.aws_stage
    url='s3://bucketsnowflakes3'
    credentials=(aws_key_id='ABCD_DUMMY_ID' aws_secret_key='1234abcd_key');


// Description of external stage

DESC STAGE MANAGE_DB.external_stages.aws_stage; 
    
    
// Alter external stage   

ALTER STAGE aws_stage
    SET credentials=(aws_key_id='XYZ_DUMMY_ID' aws_secret_key='987xyz');
    
    
// Publicly accessible staging area    

CREATE OR REPLACE STAGE MANAGE_DB.external_stages.aws_stage
    url='s3://bucketsnowflakes3';

// List files in stage

LIST @aws_stage;

//Load data using copy command

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS
    FROM @aws_stage
    file_format= (type = csv field_delimiter=',' skip_header=1)
    pattern='.*Order.*';



// Creating ORDERS table

CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS (
    ORDER_ID VARCHAR(30),
    AMOUNT INT,
    PROFIT INT,
    QUANTITY INT,
    CATEGORY VARCHAR(30),
    SUBCATEGORY VARCHAR(30));
    
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS;
   
// First copy command

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS
    FROM @aws_stage
    file_format = (type = csv field_delimiter=',' skip_header=1);




// Copy command with fully qualified stage object

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS
    FROM @MANAGE_DB.external_stages.aws_stage
    file_format= (type = csv field_delimiter=',' skip_header=1);




// List files contained in stage

LIST @MANAGE_DB.external_stages.aws_stage;    




// Copy command with specified file(s)

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS
    FROM @MANAGE_DB.external_stages.aws_stage
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails.csv');
    



// Copy command with pattern for file names

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS
    FROM @MANAGE_DB.external_stages.aws_stage
    file_format= (type = csv field_delimiter=',' skip_header=1)
    pattern='.*Order.*';
    

```


```
 ---- Assignment solution - Create stage & load data ----
 
-- create stage object
CREATE OR REPLACE STAGE EXERCISE_DB.public.aws_stage
    url='s3://snowflake-assignments-mc/loadingdata';

-- List files in stage
LIST @EXERCISE_DB.public.aws_stage;

-- Load the data 
COPY INTO EXERCISE_DB.PUBLIC.CUSTOMERS
    FROM @aws_stage
    file_format= (type = csv field_delimiter=';' skip_header=1)
```


```
// Transforming using the SELECT statement

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM (select s.$1, s.$2 from @MANAGE_DB.external_stages.aws_stage s)
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files=('OrderDetails.csv');



// Example 1 - Table

CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID VARCHAR(30),
    AMOUNT INT
    )
   
   
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX;
   
// Example 2 - Table    

CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID VARCHAR(30),
    AMOUNT INT,
    PROFIT INT,
    PROFITABLE_FLAG VARCHAR(30)
  
    )


// Example 2 - Copy Command using a SQL function (subset of functions available)

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM (select 
            s.$1,
            s.$2, 
            s.$3,
            CASE WHEN CAST(s.$3 as int) < 0 THEN 'not profitable' ELSE 'profitable' END 
          from @MANAGE_DB.external_stages.aws_stage s)
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files=('OrderDetails.csv');


SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX


// Example 3 - Table

CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID VARCHAR(30),
    AMOUNT INT,
    PROFIT INT,
    CATEGORY_SUBSTRING VARCHAR(5)
  
    )


// Example 3 - Copy Command using a SQL function (subset of functions available)

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM (select 
            s.$1,
            s.$2, 
            s.$3,
            substring(s.$5,1,5) 
          from @MANAGE_DB.external_stages.aws_stage s)
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files=('OrderDetails.csv');


SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX


```


### more transformation
```
//Example 3 - Table

CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID VARCHAR(30),
    AMOUNT INT,
    PROFIT INT,
    PROFITABLE_FLAG VARCHAR(30)
  
    )



//Example 4 - Using subset of columns

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX (ORDER_ID,PROFIT)
    FROM (select 
            s.$1,
            s.$3
          from @MANAGE_DB.external_stages.aws_stage s)
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files=('OrderDetails.csv');

SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX;



//Example 5 - Table Auto increment

CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID number autoincrement start 1 increment 1,
    AMOUNT INT,
    PROFIT INT,
    PROFITABLE_FLAG VARCHAR(30)
  
    )



//Example 5 - Auto increment ID

COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX (PROFIT,AMOUNT)
    FROM (select 
            s.$2,
            s.$3
          from @MANAGE_DB.external_stages.aws_stage s)
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files=('OrderDetails.csv');


SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX WHERE ORDER_ID > 15;


    
DROP TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX
```
