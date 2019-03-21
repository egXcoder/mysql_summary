# Some Notes of MYSQL

## DBMS : Database Management System
> the technology of storing and retrieving users’ data with utmost efficiency for example MYSQL, PostegreSQL, SqlLite, MariaDb, SAP, Oracle ... and so on

---
## MYSQL is ( Relational Database )
1. [Storage_engines](#1-Storage_engines)
1. [Data_Types](#2-data_types)
   
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
  * INT: Normal Sized Integer width up to 11 digits
  * INT(3): allocate memory for 3 bytes but inserting value which is bigger,SQL will try to get more memory from the system.
  * TinyInt:very small int -127 to 127
  * SmallInt:-32k to 32k
  * MediumInt:-8M to 8M
  * BigInt:up to 20 digits
  * Float .. estimated percetion with large number 1.5e5
  * Double..estimated percition with digits such 1.56214 (best practice for digits)
  * Decimal(5,2)..number is 5 length with 2 numbers after digit...used with math operation for perfect percetion.


> unsigned  means don't allow negatives while signed allow negative and positive (signed is the default) it can be put before any datatype


* ### String:
  * CHAR: is a fixed length string
  * VARCHAR(length):Variable length of string with specific length
  * TEXT is good:
    * If you need to store large texts in your database
    * If you do not search on the value of the column
    * If you select this column rarely and do not join on it.
     <br>Some examples of what TEXT is good for: Blog comments , Page Reviews , Content Description

* ### Date and Time:
  * Date: YYYY-MM-DD
  * TimeStamp: YYYY MM DD HH:MM:SS from 1970 to 2038 (best pracice)
  * DateTime: YYYY-MM-DD HH:MM:SS from 1000 to 9999
