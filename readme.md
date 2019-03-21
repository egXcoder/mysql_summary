# Some Notes of MYSQL

## DBMS : Database Management System
> the technology of storing and retrieving users’ data with utmost efficiency for example MYSQL, PostegreSQL, SqlLite, MariaDb, SAP, Oracle ... and so on

---
## MYSQL is ( Relational Database )
1. [Storage_engines](#storage_engines)
1. [Data_Types](#data_type)
## 1. Storage_engines :

> is what mysql uses to store, handle and retreive data and there are many supported .. you can see the supported one from "SHOW ENGINES" query

we can specifiy engine for every table to have different engine

```SQL
    CREATE TABLE users (column_name VARCHAR(200)) ENGINE=MyISAM;
    CREATE TABLE posts (column_name VARCHAR(200)) ENGINE=INNODB;
```

### Some Different engines:
* ####  MYISAM : 
it supports full text searches index(take a phrase and match it with a column) ,Table level blocking which means it block table from any another query while inserting and updating, its great for site with low insert/update rate and a very high select rate 

>Full Text searching can be enabled in InnoDB
```SQL
/* for large DB, full text search is tons faster */

/* this will add fullText feature in title column */
ALTER TABLE items ADD FULLTEXT(title);

/* checking for entries haveing 'watch' but don't have 'smart' in same time */
SELECT title FROM items WHERE match(title) Against ("+watch -smart" IN BOOLEAN MODE);
 
```

* #### InnoDB :
  
    * its the default one now, and it has many features over myisam.
    * it uses row-level blocking.
    * it allows parallel insert/update/delete queries to work in parallel on table
    * it has foreign key functionality which is advantage
  

<br><img src="https://i.ibb.co/GxNTFg7/foreign-Key-intro.png" alt="foreignKey_intro"/>

* Activity.winner is pointing to Student_ID,
* Every entry in activity.winner should exist in Student_ID otherwise it won't allow this entry
* Student_ID and Activity.winner should have the same DataType 
* If we removed S1 then the record in activity which point to S1 will be removed also automatically 


## 2. Data_Types

* ###  Numerical :
> unsigned  means don't allow negatives while signed allow negative and positive (signed is the default) it can be put before any datatype


