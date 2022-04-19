# Snowflake resource
https://docs.snowflake.com/en/user-guide/intro-key-concepts.html#data-platform-as-a-cloud-service

### Architecture

So here we see these three layers that snowflake basically consists of. **So the first layer is the storage.** So this is where our data is stored.

Of course, we know that the data is not stored in Snowflake itself, but for example, in our case, this is stored in Amazon AWS S3 buckets.

And this is also now not that important technically, but in this case, this data is stored in hybrid Columbanus storage.

So in contrast to the traditional form of storing data, we store the data instead of saving it in rows, we store them and compress them into blobs.

And then when we query the data, we also don't fetch these rows, but we fetch these blobs.

And this hybrid column, not storage, is used in a lot of big data contexts. And so it is also in here. And this makes it more efficient in storing the data and also faster even in querying this data.

**The second layer is virtual warehouse**. This is different from data warehouse. Muscle of the system performs MMP

**Brain of the system** is cloud services. Managing infrastructure, access control. 

XS virtual wareshouse -> 1 servers
S virtual warehourse -> 2 servers
M virtual warehouse -> 4 servers

multiclustering -> for example multiple S virtual warehouses  when necessary

### create data warehouses
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

Standard: minimizes queuing by favoring starting additional clusters over conserving credits. Cluster starts immediately when either a query is queued or the system detects that there are more queries than can be executed by the currently available clusters. Cluster shuts down after 2-3 consecutive successful checks(performs every 1 minute). checks determine whether the load on the least-loaded cluster could be redistributed to other clusters. 


Economy: conserves credits by favoring keeping running clusters fully-loaded rather than starting additional clusters. Clusters starts only if the system estimates there's enough query load to keep the cluster busy at least 6 minutes. Cluster shut down after 5-6 consecutive successful checks. 



### create database and tables
```
CREATE DATABASE OUR_FIRST_DB;

CREATE TABLE "OUR_FIRST_DB"."PUBLIC"."SECOND_TABLE" (
 "AGE_CUSTOMER" INTEGER NOT NULL,
 "CUSTOMERNAME" STRING NOT NULL
)
```

### list schema and table

```
show schemas in database our_first_db;
show tables in database our_first_db;
show tables in schema our_first_db.INFORMATION_SCHEMA;

```

### snowflake pricing
standard $2/credit
enterprise $3/credit
business critical $4/credit
virtual private contact snowflake

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


### copy on error

```
 // Create new stage
 CREATE OR REPLACE STAGE MANAGE_DB.external_stages.aws_stage_errorex
    url='s3://bucketsnowflakes4'
 
 // List files in stage
 LIST @MANAGE_DB.external_stages.aws_stage_errorex;
 
 
 // Create example table
 CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID VARCHAR(30),
    AMOUNT INT,
    PROFIT INT,
    QUANTITY INT,
    CATEGORY VARCHAR(30),
    SUBCATEGORY VARCHAR(30));
 
 // Demonstrating error message
 COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv');
    

 // Validating table is empty    
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX    
    

  // Error handling using the ON_ERROR option
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv')
    ON_ERROR = 'CONTINUE';
    
  // Validating results and truncating table 
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX
SELECT COUNT(*) FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX

TRUNCATE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX;

// Error handling using the ON_ERROR option = ABORT_STATEMENT (default)
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv','OrderDetails_error2.csv')
    ON_ERROR = 'ABORT_STATEMENT';


  // Validating results and truncating table 
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX
SELECT COUNT(*) FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX

TRUNCATE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX;

// Error handling using the ON_ERROR option = SKIP_FILE
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv','OrderDetails_error2.csv')
    ON_ERROR = 'SKIP_FILE';
    
    
  // Validating results and truncating table 
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX
SELECT COUNT(*) FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX

TRUNCATE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX;    
    

// Error handling using the ON_ERROR option = SKIP_FILE_<number>
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv','OrderDetails_error2.csv')
    ON_ERROR = 'SKIP_FILE_2';    
    
    
  // Validating results and truncating table 
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX
SELECT COUNT(*) FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX

TRUNCATE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX;    

    
// Error handling using the ON_ERROR option = SKIP_FILE_<number>
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv','OrderDetails_error2.csv')
    ON_ERROR = 'SKIP_FILE_0.5%'; 
  
  
SELECT * FROM OUR_FIRST_DB.PUBLIC.ORDERS_EX


 CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID VARCHAR(30),
    AMOUNT INT,
    PROFIT INT,
    QUANTITY INT,
    CATEGORY VARCHAR(30),
    SUBCATEGORY VARCHAR(30));





COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv','OrderDetails_error2.csv')
    ON_ERROR = SKIP_FILE_3 
    SIZE_LIMIT = 30;




```

### file format

```

// Specifying file_format in Copy command
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format = (type = csv field_delimiter=',' skip_header=1)
    files = ('OrderDetails_error.csv')
    ON_ERROR = 'SKIP_FILE_3'; 
    
    

// Creating table
CREATE OR REPLACE TABLE OUR_FIRST_DB.PUBLIC.ORDERS_EX (
    ORDER_ID VARCHAR(30),
    AMOUNT INT,
    PROFIT INT,
    QUANTITY INT,
    CATEGORY VARCHAR(30),
    SUBCATEGORY VARCHAR(30));    
    
// Creating schema to keep things organized
CREATE OR REPLACE SCHEMA MANAGE_DB.file_formats;

// Creating file format object
CREATE OR REPLACE file format MANAGE_DB.file_formats.my_file_format;

// See properties of file format object
DESC file format MANAGE_DB.file_formats.my_file_format;


// Using file format object in Copy command       
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (FORMAT_NAME=MANAGE_DB.file_formats.my_file_format)
    files = ('OrderDetails_error.csv')
    ON_ERROR = 'SKIP_FILE_3'; 


// Altering file format object
ALTER file format MANAGE_DB.file_formats.my_file_format
    SET SKIP_HEADER = 1;
    
// Defining properties on creation of file format object   
CREATE OR REPLACE file format MANAGE_DB.file_formats.my_file_format
    TYPE=JSON,
    TIME_FORMAT=AUTO;    
    
// See properties of file format object    
DESC file format MANAGE_DB.file_formats.my_file_format;   

  
// Using file format object in Copy command       
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM @MANAGE_DB.external_stages.aws_stage_errorex
    file_format= (FORMAT_NAME=MANAGE_DB.file_formats.my_file_format)
    files = ('OrderDetails_error.csv')
    ON_ERROR = 'SKIP_FILE_3'; 


// Altering the type of a file format is not possible
ALTER file format MANAGE_DB.file_formats.my_file_format
SET TYPE = CSV;


// Recreate file format (default = CSV)
CREATE OR REPLACE file format MANAGE_DB.file_formats.my_file_format


// See properties of file format object    
DESC file format MANAGE_DB.file_formats.my_file_format;   



// Truncate table
TRUNCATE table OUR_FIRST_DB.PUBLIC.ORDERS_EX;



// Overwriting properties of file format object      
COPY INTO OUR_FIRST_DB.PUBLIC.ORDERS_EX
    FROM  @MANAGE_DB.external_stages.aws_stage_errorex
    file_format = (FORMAT_NAME= MANAGE_DB.file_formats.my_file_format  field_delimiter = ',' skip_header=1 )
    files = ('OrderDetails_error.csv')
    ON_ERROR = 'SKIP_FILE_3'; 

DESC STAGE MANAGE_DB.external_stages.aws_stage_errorex;

```


```
 ---- Assignment - Create file format & load data ----
 
-- create stage object
CREATE OR REPLACE STAGE EXERCISE_DB.public.aws_stage
    url='s3://snowflake-assignments-mc/fileformat';

-- List files in stage
LIST @EXERCISE_DB.public.aws_stage;

-- create file format object
CREATE OR REPLACE FILE FORMAT EXERCISE_DB.public.aws_fileformat
TYPE = CSV
FIELD_DELIMITER='|'
SKIP_HEADER=1;

-- Load the data 
COPY INTO EXERCISE_DB.PUBLIC.CUSTOMERS
    FROM @aws_stage
      file_format= (FORMAT_NAME=EXERCISE_DB.public.aws_fileformat)
      
-- Alternative
COPY INTO EXERCISE_DB.PUBLIC.CUSTOMERS
    FROM @aws_stage
      file_format= EXERCISE_DB.public.aws_fileformat
```
### clusters

snowflake is creating cluster keys on certain columns to create subset of rows to locate them  in micro-partions

* snowflake automatically maintains these cluster keys
* in general snowflake produces well-clustered tables
* cluster keys are not always ideal and can change over time
* manually customize these cluster keys

will benifit multiple terabyte data tables

* how to cluster?    
columns that are used frequently in where clauses (often date columns for event tables)

if you typically filters on two columns then the table can also benefit from two cluster keys

column that is frequently used in joins

Large enough number of distinct values to enable effective grouping(too large not a good cluster key)

Small enough number of distinct values to allow effective grouping(too small not a good cluster key)



### Best practice
* Enable auto-suspend
* Enable auto-resume
* Set appropriate timeouts
  * ETL/Data loading => timeout immidiately
  * BI/SELECT queries => timeout 10 min
  * Devops/Data science => timeout 5 min


* If we have very complex queries that take a lot of time, it is no problem at all to set a larger virtual warehouse size. And then we can also later on, if we see this enough, we can also decrease the size of the virtual warehouse.

So we should not focus too much on using a small virtual warehouse because we always can use: enable auto suspend.

And usually, if we use a virtual warehouse that is large for a very complex query, then this is not a lot more in terms of cost because it can also process these queries a lot faster.

So these were the best practices regarding the virtual warehouse.

* Appropriate table type
  * staging tables - Transient
  * Production tables - Permanent
  * Development tables - transient


* Appropriate table type  
* set cluster keys only if necessary
  * Large table
  * Most query time for table scan
  * dimentions

### monitoring

```

-- Table Storage

SELECT * FROM "SNOWFLAKE"."ACCOUNT_USAGE"."TABLE_STORAGE_METRICS";

-- How much is queried in databases
SELECT * FROM "SNOWFLAKE"."ACCOUNT_USAGE"."QUERY_HISTORY";

SELECT 
DATABASE_NAME,
COUNT(*) AS NUMBER_OF_QUERIES,
SUM(CREDITS_USED_CLOUD_SERVICES)
FROM "SNOWFLAKE"."ACCOUNT_USAGE"."QUERY_HISTORY"
GROUP BY DATABASE_NAME;


-- Usage of credits by warehouses
SELECT * FROM "SNOWFLAKE"."ACCOUNT_USAGE"."WAREHOUSE_METERING_HISTORY";

-- Usage of credits by warehouses // Grouped by day
SELECT 
DATE(START_TIME),
SUM(CREDITS_USED)
FROM "SNOWFLAKE"."ACCOUNT_USAGE"."WAREHOUSE_METERING_HISTORY"
GROUP BY DATE(START_TIME);

-- Usage of credits by warehouses // Grouped by warehouse
SELECT
WAREHOUSE_NAME,
SUM(CREDITS_USED)
FROM "SNOWFLAKE"."ACCOUNT_USAGE"."WAREHOUSE_METERING_HISTORY"
GROUP BY WAREHOUSE_NAME;

-- Usage of credits by warehouses // Grouped by warehouse & day
SELECT
DATE(START_TIME),
WAREHOUSE_NAME,
SUM(CREDITS_USED)
FROM "SNOWFLAKE"."ACCOUNT_USAGE"."WAREHOUSE_METERING_HISTORY"
GROUP BY WAREHOUSE_NAME,DATE(START_TIME);

```

### Retention period
Best practice: 

Staging database - 0 days (transient)

Production - 4-7 days (1 day min)

Best practice #3 - Large high-churn tables - 0 days (transient)


### Access Control
Discretionary access control (DAC): Each object has an owner who can grant access to that object

Role-based access control (RBAC): Access priviledges are assigned to roles, which are in turn assigned to users

Example: 

Role 1 creates a table. This means Role 1 owns this table. 

Role 1 can grant the privileges to role 2 and role 3. 

Role 2 Role 3 etc can we assigned to different users. 

```
GERANT <privilege> on <object> TO <role>

GRANT <role> to <user>
```

### Access Control
Account object has: 

User, Role, Database, Warehouse, Other Account objects

Database object has: 

schema

Schema object has: 

Table, View, Stage, Integration, other schema objects

Every object owned by a single role, the role can be assigned to multiple users
Owner(role) has all privileges per default)


### Hirarchy of roles

So if the role **public** has a user with the role public has created a table, then these privileges will be also inherited to the other roles according to this hierarchy.

### Role concepts

1. User: People or systems
2. Role: Entity to with priviledges are granted
3. Privilege: Level of access to an object(select, drop, create, etc.)
4. Securable object: objects to which privileges can be granted on (Database, Table, Warehouse etc.)


### AccountAdmin
* inherite both sysadmin and securityadmin
* top-level role in the system
* should be grated only to a limited number of users

### securityadmin
* useradmin role is granted(inherited by) to securityadmin
* can manage users and roles
* can manage any object grant globally

### Systemadmin
* create warehouses and databases(more objects)
* recommended that all custome roles are assigned

### Useradmin
* dedicated to user and role management only
* can create users and roles

### public
* automatically granted to every user
* can create own objects like every other role(the object is available to every other user/role)

### accountadmin
* top-level-role
* manage & view all objects
* all configurations on account level

**Usage**:
First user will have this role assigned

initial setup & managing account level objects


**Best practises**
* Very controlled(careful) assignment strongly recommented
* Multi-factor authentification
* at least two users should be assigned that role
* avoid creating objects with that role unless you have to 


```

--- User 1 ---
CREATE USER maria PASSWORD = '123' 
DEFAULT_ROLE = ACCOUNTADMIN 
MUST_CHANGE_PASSWORD = TRUE;

GRANT ROLE ACCOUNTADMIN TO USER maria;


--- User 2 ---
CREATE USER frank PASSWORD = '123' 
DEFAULT_ROLE = SECURITYADMIN 
MUST_CHANGE_PASSWORD = TRUE;

GRANT ROLE SECURITYADMIN TO USER frank;


--- User 3 ---
CREATE USER adam PASSWORD = '123' 
DEFAULT_ROLE = SYSADMIN 
MUST_CHANGE_PASSWORD = TRUE;
GRANT ROLE SYSADMIN TO USER adam;
```

### security admin
```
-- SECURITYADMIN role --
--  Create and Manage Roles & Users --


-- Create Sales Roles & Users for SALES--

create role sales_admin;
create role sales_users;

-- Create hierarchy
grant role sales_users to role sales_admin;

-- As per best practice assign roles to SYSADMIN
grant role sales_admin to role SYSADMIN;


-- create sales user
CREATE USER simon_sales PASSWORD = '123' DEFAULT_ROLE =  sales_users 
MUST_CHANGE_PASSWORD = TRUE;
GRANT ROLE sales_users TO USER simon_sales;

-- create user for sales administration
CREATE USER olivia_sales_admin PASSWORD = '123' DEFAULT_ROLE =  sales_admin
MUST_CHANGE_PASSWORD = TRUE;
GRANT ROLE sales_admin TO USER  olivia_sales_admin;

-----------------------------------

-- Create Sales Roles & Users for HR--

create role hr_admin;
create role hr_users;

-- Create hierarchy
grant role hr_users to role hr_admin;

-- This time we will not assign roles to SYSADMIN (against best practice)
-- grant role hr_admin to role SYSADMIN;


-- create hr user
CREATE USER oliver_hr PASSWORD = '123' DEFAULT_ROLE =  hr_users 
MUST_CHANGE_PASSWORD = TRUE;
GRANT ROLE hr_users TO USER oliver_hr;

-- create user for sales administration
CREATE USER mike_hr_admin PASSWORD = '123' DEFAULT_ROLE =  hr_admin
MUST_CHANGE_PASSWORD = TRUE;
GRANT ROLE hr_admin TO USER mike_hr_admin;

```

### sysadmin

```
-- SYSADMIN --

-- Create a warehouse of size X-SMALL
create warehouse public_wh with
warehouse_size='X-SMALL'
auto_suspend=300 
auto_resume= true

-- grant usage to role public
grant usage on warehouse public_wh 
to role public

-- create a database accessible to everyone
create database common_db;
grant usage on database common_db to role public

-- create sales database for sales
create database sales_database;
grant ownership on database sales_database to role sales_admin;
grant ownership on schema sales_database.public to role sales_admin

SHOW DATABASES;


-- create database for hr
drop database hr_db;
grant ownership on database hr_db to role hr_admin;
grant ownership on schema hr_db.public to role hr_admin

```

### When to use materialized view
a good use case: takes a long time to query; is updated rarely, is queried often


* don't use materialized view if data changes are very frequent   
* keep maintenance cost in mind   
* consider leveraging tasks & streams instead   

only available in enterprice version

joins (including self-joins ) are not supported

limited amount of aggregation functions

no user defined functions
no having clauses
no order by clause
no limit clause


### Data sharing
* Usually this can be a rather complicated process
* data sharing without actual copy of that data & up to date
* shared data can be consumed by the own compute resources
* non-snoflake-users can also access through a reader account
