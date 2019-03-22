# Some Notes of MYSQL

## DBMS : Database Management System
> the technology of storing and retrieving users’ data with utmost efficiency for example MYSQL, PostegreSQL, SqlLite, MariaDb, SAP, Oracle ... and so on

---
## MYSQL is ( Relational Database )
1. [Storage_engines](#1-Storage_engines)
1. [Data_Types](#2-data_types)
1. [Functional_Dependecy](#3-Functional-Dependency) 
1. [Normalization](#4-Normalization) 
   
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
  

      <br><img src="images/foreignKey_intro.png"/>

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
  * Decimal(5,2)..number is 5 length with 2 numbers after digit...used with math operation for perfect percetion.<br><br>


   > unsigned  means don't allow negatives while signed allow  negative and positive (signed is the default) it can be put before any datatype


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

## 3. Functional Dependency:
A column is dependent on another one if one value can be used to determine the value of another
Example:first Name is dependent on id , since by id we can determine value of firstName

### **Foreign Key in pracice**
Adding foreign key to make products.category to point to categories.id

Categories Table

| id  | name     |
| --- | -------- |
| 1   | phones   |
| 2   | laptops  |
| 3   | clothes  |
| 4   | machines |

Products Table

| id  | name           | description | price | category | image | created_date |
| --- | -------------- | ----------- | ----- | -------- | ----- | ------------ |
| 1   | oppo phone     |             | 100$  | 1        |       |              |
|     | toshiba laptop |             | 500$  | 2        |       |              |
|     | watch          |             | 50$   | 4        |       |              |
|     | samsung phone  |             | 150$  | 1        |       |              |

* ### adding index to products.category column
   
    <img src="images/foreign_key_1.png" />

* ### click on relation view
    
    <img src="images/foreign_key_2.png" />

* ### make products.category column point to categories.id

    <img src="images/foreign_key_3.png" />

* ### Restrict:
    means if we want to remove a category from the categories Table, it won't allow removal because there is a product pointing to it

* ### Cascade:
     means if we want to remove a category from the categories Table, it will remove all the products pointing to this category

* ### Null:
  means if we want to remove a category from the categories Table,it will reset the value of product.category which point to parent category with null

* ### No actions:
  means if we want to remove a category from the categories Table,nothing will happen in products table even if there are some pointing to parent category

## 4. Normalization:
 
 is the process of organizing the fields and tables to minimize redudancy and control dependency.

 we can normalize data by three filters (normal forms)

* **1NF:** every column should represents single attribute (don't have multiply value in single column) for example the below table breaks 1NF
  
| user_id | user_name             |
| ------- | --------------------- |
| 1       | ahmed,mohamed,ibrahim |
| 2       | alaa                  |
| 3       | sarah,nadia           |
| 4       | john                  |

<br>
<br>

* **2NF:** all columns inside a table should depend on the pk, if a column is partial depend on one PK but not the other such in in the below table (product_description) (its better place is in product table)
notice:2 PK in one table is called (Composite Key)

users Table , product_table and orders_table 

| user_id | user_name |     |     | product_id | product_name |     |     | id  | user_id | product_id | product_description     |
| ------- | --------- | --- | --- | ---------- | ------------ | --- | --- | --- | ------- | ---------- | ----------------------- |
| 1       | ahmed     |     |     | 1          | watch        |     |     | 1   | 1       | 1          | its samsung smart watch |
| 2       | alaa      |     |     | 2          | perfum       |     |     | 2   | 1       | 2          | exciting perfum         |
| 3       | sarah     |     |     | 3          | book         |     |     | 3   | 3       | 2          | exciting perfum         |
| 4       | john      |     |     | 4          | PC           |     |     | 4   | 4       | 2          | exciting perfum         |

<br>
<br> 

* **3NF:** if column depend on another column in the table, its better to break it to another table and make your references., in the below table category_description depends on category column,so itsn't belong to this table

| product_id | product_name        | category | category_description     |
| ---------- | ------------------- | -------- | ------------------------ |
| 1          | samsung smart watch | watches  | a very effecient watches |
| 2          | dell inspiron 5570  | Laptops  | high performance laptops |
| 3          | Lean Startup        | Books    | inspiring books          |
| 4          | Toshiba L755        | Laptops  | high performance laptops |

the fix is to create another table for category Table and make a foreign key between products.category_id to category.id

| product_id | product_name        | Category_id |     |     | id  | category | category_description     |
| ---------- | ------------------- | ----------- | --- | --- | --- | -------- | ------------------------ |
| 1          | samsung smart watch | 1           |     |     | 1   | watches  | a very effecient watches |
| 2          | dell inspiron 5570  | 2           |     |     | 2   | Laptops  | high performance laptops |
| 3          | Lean Startup        | 3           |     |     | 3   | Books    | inspiring books          |
| 4          | Toshiba L755        | 2           |     |     | 4   | PC       | personal computers       |

* ### Common Design Mistakes

  * Tables with many field that don't relate to eachother
  * Too many tables with similar data
  * repeated rows (thats why we use primary key)
  * using comma separated value in a column
  * poor naming conventions
  * non-normalized data

* ### Database Design Process:
  * define the purpose of the database, we want create db for website that will
    * sell products which can be categorized
    * create customer accounts
    * allow customers to create reviews for products
    * provide basic content management system for static pages

  * now determine your table names,and column_ids and use name conventions on it
    * product_categories
    * products
    * customers
    * reviews
  
  * normalize the database by finding relations between tables
