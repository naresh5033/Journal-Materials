# Sql (FCC)

- as we know the relational db relates different entities in diff tables.
- the big diff b/w the relational and the non relational db is that the non relational db nest their data instead of keeping the records on the separate table, they "store the record with in their records".. to simplify this we can think of the relational db as the giant **Blob of nested Json**
- sql, postgre, oracle, mysql, cockroach db, sql server all uses sql lang.
- to create a table.. CREATE TABLE users(id INTEGER, name TEXT, email TEXT, )
- to inssert val in to the talbe.. INSERT into users(id, name, email) values (1, 'John')
- to alter the table.. ALTER TABLE users // RENAME TO employees;
- lly to alter the table's column ALTER TABLE users // RENAME COLUMN salary TO invoice
- then add or drop a column ALTER TABLE users // ADD COLUMN job_title TEXT; // for the drop - ALTER TABLE users DROP COLUMN job_description;

### DB Migrations

- the act of adding new column is migration, is safe
- and deleting the column is not safe.
- for update its good practice to take the copy of the old data and make it new then we can deploy the new copy ..

  - or we can aliasing the old tale (give the old table) a new name the old code reference the old name and the new code reference the new name.
  - once we safely deploy the new code we can delete the old one .. which is safer.

- **Up and Down Migrations** - any migrations that brings forward in time is called up migrations.
- the down migration is used to roll back changes in the case of emergency. or something goes wrong.
  - ex of Up migrations are - 1. create a new table 2, del table 3.Create table 4. Rename column 5. Delete Column 6
  - ex of Down migrations are - this is just inverse of the Up migration (whatever mentioned above, its just the inverse)

### Data Types

- Null, integer, boolean(0- false, 1- true), real (64 bit float), text(stored in db encoded as utf-8), Blob (binary large object) typically used for multi media, like imgs, audio, video.

## Constraints

- when creating a table, we can define whether or not the field can or can't be null that's a kind of constraint
- **Null val** in sql cell the null value indicates the val is **missing**
- the not null constraint can be added while creating the table ex - CREATE TABLE users { id INTEGER PRIMARY KEY, name TEXT UNIQUE, age INTEGER NOT NULL };

  - we can think of primary key constraints as the unique constraint, not null

- **Primary Key** defines and protects the relationship between the table, each table can ve one and only primary key. most of the time it will be the id column of the DB and that will be the primary key.

- **Foreign Key** is the one which makes the relational DB a **relational** foreign key is the one which defines the relationship b/w the tables - foreign key is the field in which one tale reference the other table's primary key..
  ex - CREATE TABLE department {id INTEGER PRIMARY KEY, name TEXT} ; CREATE TABLE employee {id INTEGER PRIMARY KEY, name TEXT, department_id INTEGER CONSTRAINT fk_department FOREIGN KEY (department_id) REFERENCES department(id)};

- **Auto INCREMENT** will automatically increment the vals in a column. ex in a id column.. in most db's they ve it by default ex - sql lite..

- **COUNT** we can use select count to get the count of the records in a table. ex - SELECT count(\*) FROM employee

### CRUD

- **WHERE Clause** using the WHERE clause to specify which records to retrieve.. ex - SELECT name FROM users WHERE power_level >= 1000;

- **Finding null values** we can use the where clause to filter whether or not the values are null
  - **IS NULL** ex - SELECT name FROM users WHERE first_name IS NULL;
  - **IS NOT NULL** ex - SELECT name FROM users WHERE first_name IS NOT NULL;
- **Delete Statement** ve to be careful when dealing with this delete statement.

  - to delete everything DELETE FROM users; - will delete all records in the user table.
  - or to delete specific record ex - DELETE FROM users WHERE id = 232;

- most of the companies will take a snapshot of their prod db on the daily basis or weekly basis, which is safer for them to recover or rollback if some thing happens.
  **soft deletes** - means we never ve to delete from the database, instead if we don't need a record, we can simply mark em as **deleted_at** date on row..

- **update statement** to update the fields in the record ex - SELECT users SET job_title = 'dev', salary = 12334 WHERE id = 223;

### ORM

- is a tool that allow us to perform CRUD operations on a db using the traditional programming language..
- the primary benefit of the orm provides is that it maps the our database records to in memory
- using the orm it makes our code less verbose and we don't ve to write more code.. however it doen't give more control overt the db.
  - depends on the project we can decide whether or not to use an orm.

### Basic Queries

- **AS clause** is like the alias (or assertions). note: this alias only exists for the duration of the query.. it will not save to the db.
- ex - SELECT username AS name, user_id AS id FROM users; its kind of useful in structuring the return data standpoint..

- **sql function** there are few functions that are available in sql.

  - **IIF** is like a ternary operator.. if the condition is true return the 1st val otherwise return the 2nd val.
  - ex - SELECT quantity, IIF(quantity < 10, "order more", "inStock") AS directive FROM products

- **Between clause** to check whether the val is in b/w 2 numbers. - ex SELECT employee_name, salary FROM employee WHERE salary BETWEEN 10000 and 20000;

  - lly we can use NOT along side with the b/w clause, everything that is not b/w the 2 numbers.

- **DISTINCT** is useful kw which allow us to select the rows with the distinct vals. means removes the duplicate record from the resulting query.
  - ex - SELECT DISTINCT employee_name FROM employee

## logical operators

- **And** ex - SELECT product_name, quantity FROM product WHERE product_name = 'apple' AND quantity BETWEEN 20000 AND 20000
- **Equality operator** ex - =, <, >, <=, >= ;
- **OR** lly as the and choose either one instead of both.

- **IN** this clause returns true or false, if the 1st operand matches any vals in the second operand.

  - ex SELECT product_name, shipment_status FROM product WHERE shipment_status IN ('shipped', 'prepared', 'ready'); // it is like using or operator 3 times in the shipment_status, it is convenient.

- **Like operator** allow us to use the fuzzy matching, or wild card matching. the LIKE kw allow us to use of the % and the \_ wildcard operators.

  - **% operator** this will match 0 or more characters, we can use this in our qs to find more than just exact matches
  - ex - SELECT \* FROM product WHERE product_name like 'apple%' - for any products starts with the name apple;
  - SELECT \* FROM product WHERE product_name like '%apple' - for any products ends with the name apple.
  - SELECT \* FROM product WHERE product_name like '%apple%' - for product name contains any where the name apple.
  - **\_ underscore** mean while the along side with the like kw this \_ only matches a single character
    - ex - SELECT \* FROM product WHERE product_name like '\_oot'; will matches like boot, root, loot etc
    - double ** ex - SELECT \* FROM product WHERE product_name like '**oot'; // will matches like shoot, broot, groot.

- ### Structuring
- **Limit** kw can be used in the end of the statement to reduce the number of returned records.

  - can be very handy in a huge db with similar search we can use the LIMIT keyword at the end ex - LIMIT 50; // returns the qs matches with the limit of 50 records.

- **order by** this clause go hand in hand with the limit clause. this kw offers ability to sort the result either by ascending or Descending order. by default its in ascending order.
  - ex SELECT name, price FROM products ORDER BY name desc ; // for descending order we ve to explicitly mention.

### Aggregations

- it takes the large amount of data and aggregates em in a single val. as the data stored in the db are in raw format we need to do some calculation we can use aggregations,
- ex - SELECT COUNT(\*) FROM products WHERE quantity = 0;
- **sum** this aggregate fn will sums up the group of vals. ex - SELECT sum(salary) FROM employees;
- **max** returns the largest val in the group/record ex - SELECT max(salary) FROM employees;
- **min** lly the min returns the smallest/ min val.

- **Group By** gives powerful way to interact with the aggregate fn. it allows multiple summary rows and the interesting part is each group can ve an aggregate fn apply to it
  - ex - SELECT album_id, count(song_id) FROM songs GROUP BY album_id; // this query returns count of all the songs on each album and one record is returned per album and they each ve their own count.
- **Average** returns the average val avg()// just like the min and max

- **HAVING clause** - this is sim to where clause, it specifies the search condition of the group.
  - this is just like the where clause it filters down the rows but the diff is having operates on row after, the aggregation has taken in place. where as the where clause operates on b4 the aggregation takes place.
  - the having clause is not as commonly used as the where clause.
  - ex - SELECT album_id, count(id) as count FROM songs GROUP BY album_id HAVING count > 4;
- - - the diff is fairly simple b/w the where clause and the having clause.
    - the where condition is applied to all data in a query b4 its grouped by a group by clause
    - the having condition is applied only the grouped row that are returned after the group by clause is applied
    - **Round** round() to round the numbers. it is common to use the round() on the result of the aggregation.

### sub queries

- some time a single query is not enough to retrieve the specific records we need, it is possible to run the query on the result set of the query.. a query within a query, this is called subquery
- it can be useful when accessing the data that can't be retrieved by a simple querying a single table.
- ex - SELECT id, song_name, artist_id FROM songs WHERE artist_id IN (SELECT id FROM artists WHERE artist_name LIKE '%ricks'); // where we want the artist id to be in the result of that following subquery. so the subquery (inside the paranthesis) will run first and the results will be used in the **IN** clause
  - in the above example we don't know the artist_id so we do the subquery in the artist table and then the result we apply the IN clause (which now has the artist id)

**No tables** As we know the sql is a programming language, we can use to interact with the database, for ex - we can select information that's simply calculated and no tables needed
ex - SELECT 5 + 10 as sum; //or SELECT _ FROM users WHERE age > (365 _ 40); // returns the users with the ate gt than 40 (in days)

### Normalization

- architect a db is really difficult, the most important thing we do is model our relationship like "one to one" or "one to many"
- there are 3 different types of entities in our database.
  1. one to many 2. one to one 3. many to many

1. one to many - the user can have many tweets the fk id on the entity ie many, in this case the tweets reference the user. (tweet id needs the user_id)
2. one to one - as we know one user has one company.
3. many to many - users and groups, users can be part of many groups and groups can ve many users. this many to many is bit complex .

- since we can't use the fk, if we use the fk then we'l stuck with the one to many relationship,
- so the approach here is to add a **new table** (lets called user_groups) and this is called the **joining table** and its purpose is to model the relationship b/w the user and the group.
- and there will be 2 things in the table user id and the group id, and we'd put the **constraints** on the table and say we re not allowed 2 rows identical on the table.

- **Joining table** the joining table usually doesn't ve any metadata(aka the prop field of an obj) on it..
- **Unique constraints** this unique constraint will makes our join table works better. ex - CREATE TABLE product_supplier (product_id INTEGER, supplier_id INTEGER), UNIQUE (product_id, supplier_id)); // this is how we model a many to many relationship..

- lets see how db normalization and certain strategies will helps us to reduce the data duplication in our db, and data integrity.. basically it makes our web apps to work easier and more maintainable
- basically we can normalize our db in **less or More** normalized way, the more normalized it is the less redundant means less "data duplication" and more data integrity(correctness/ correct data) we will ve, and lly the less normalized db means more data redundant / less integrity..
- there are 4 tiers of data normalization
  - 1. first normal form 2. second normal form 3. third normal form 4. Boyce-codd normal form..
  - as the levels goes down our database will becomes more normalized, and all those above names map some rules..

1. first Normal Form

- has 2 rules.. 1. every row must have unique primary key(literally means we can't ve completely duplicate rows),, 2. there can be no nested tables ..

2. Second Normal Form :
   - has all the rules of the first normal form plus it has its own rule ... All the column that are part of the primary key must only be dependent on the entire primary key.

3 Third Normal Form : follows all the rules of the first and second normal forms plus one additional rule.. that is

- all the columns that are not in the primary key are dependent only on the primary key..
- side note: He emphasize that the idea of the second and third normal form for the backend developer is fairly meaningless..

4. Boyce codd Normal Form: (with all the 3 rules plus)

- A column that is not part of primary key may not be dependent on the column that is not part of the primary key..
- this was implemented after they realized that still there is a way for data duplication to slip into the table even when adheres all the 3 normal forms rules.

- In the context of normalization the primary key is made up of **one to many** columns..

- note: in the real scenario we don't need to worry about these normal forms / normalizations techniques all we need to think about is not to duplicate the data,, is there a way we re restructure the table, so when we update in one place we don't wanna update in other place..
- so in summary the whole concept of the Normalization is to keep the data integrity and the data redundancy.. one ex - of data integrity is we don't ve to store the age of the user which will change so instead we can store the birthday.

### Joins

- joins are one of the most important sql features, that allows us to make use of the relationship we ve set up in b/w 2 tables.. in short joins allow us to ve query multiple tables at a time..

- **Inner Join** is the default join,, is the simplest and most common type of join.. it returns all the records(rows) of table A, that ve the matching record on the table B..
- **ON** kw is used in tandem with joins..the ON clause is used to specify these columns to joins..ex - SELECT \* FROM employees INNER JOIN department **ON** employee.department_id = department.id; // we re saying we want all of the rows in the employee table with department id that matches the row in the department table with the same id

- **Name Spacing** on tables .. when working with multiple tables, a field exist on using the . dot .. ex - SELECT student.name, classes.name FROM students INNER JOIN classes ON classes.id = student.class_id; // accessing the field or the prop of the table. by using the . dot just like other oop prog lang.

  - **Left Joins** will returns all the records in the table a (left side) regardless of whether or not the records matches in the table b. and then the table b will return the rows that only match with the table a

- **Right Joins** is not commonly used, and even sql lite doesn't event support to use this but some other sql db does.. the right join is just the left joins with order of the tables switched .. instead of using the right joins we can simply swap the order of the table names (and still use the left joins)

- **Full joins** it just returns all the rows from the both the tables.. one thing to keep in mind is that joins really don't operate on the columns they operate on the rows...

### Performance

- **sql index** is an in mem structure that ensure the queries we run on the db are performant.. the binary tree data structure is used bts to create the db indexes..
- so in big o notation, looking up using an index is an **log N lookup rather than N time lookup**
- ex - to create the index .. CREATE INDEX index_name ON table_name (column_name);
- however creating an idx is also ve a overhead of its own, for ex . when we re creating the index, we re telling the db to keep an in mem store of the data in that column.. so the index can slow down the **write speed**
- in general the best practice is not to use the index unless we ve a specific use cases/ requirement..

- **Multi column Index** - They speed up the looks of that depends on the multiple columns.

### Denormalization (for Speed)

- sometimes we will run into an issue that can only be solved by the denormalizing our db.. Means we can introduce **duplicate data** so we can speed things up.
- how the duplicate data will increase performance ? the answer is when copy the data we frequently putting into different format makes it easier to get at or easier to access....

- **sql Injection** is the very common way the hackers can attempt to cause damage or breach the database..
- ex - INSERT INTO student(name) VALUES(?); // if we leave our query like this the user can insert anything they want
- ex - INSERT INTO student(name) VALUES('robert'); DROP TABLE student;--// by allowing the user to insert anything we re opening ourselves into this kind of attack..

- to avoid this kind of attack we ve to sanitize the i/ps..means detecting this kind of crap and not allow it to happen.

- IN Go lang and other prog languages they ve the sql lib, that will do the variable interpolation putting dynamic val into our sql in a safe way.. protects sql injection.

# MySQL Cheat Sheet

> Help with SQL commands to interact with a MySQL database

## MySQL Locations

- Mac _/usr/local/mysql/bin_
- Windows _/Program Files/MySQL/MySQL *version*/bin_
- Xampp _/xampp/mysql/bin_

## Add mysql to your PATH

```bash
# Current Session
export PATH=${PATH}:/usr/local/mysql/bin
# Permanantly
echo 'export PATH="/usr/local/mysql/bin:$PATH"' >> ~/.bash_profile
```

On Windows - https://www.qualitestgroup.com/resources/knowledge-center/how-to-guide/add-mysql-path-windows/

## Login

```bash
mysql -u root -p
```

## Show Users

```sql
SELECT User, Host FROM mysql.user;
```

## Create User

```sql
CREATE USER 'someuser'@'localhost' IDENTIFIED BY 'somepassword';
```

## Grant All Priveleges On All Databases

```sql
GRANT ALL PRIVILEGES ON * . * TO 'someuser'@'localhost';
FLUSH PRIVILEGES;
```

## Show Grants

```sql
SHOW GRANTS FOR 'someuser'@'localhost';
```

## Remove Grants

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'someuser'@'localhost';
```

## Delete User

```sql
DROP USER 'someuser'@'localhost';
```

## Exit

```sql
exit;
```

## Show Databases

```sql
SHOW DATABASES
```

## Create Database

```sql
CREATE DATABASE acme;
```

## Delete Database

```sql
DROP DATABASE acme;
```

## Select Database

```sql
USE acme;
```

## Create Table

```sql
CREATE TABLE users(
id INT AUTO_INCREMENT,
   first_name VARCHAR(100),
   last_name VARCHAR(100),
   email VARCHAR(50),
   password VARCHAR(20),
   location VARCHAR(100),
   dept VARCHAR(100),
   is_admin TINYINT(1),
   register_date DATETIME,
   PRIMARY KEY(id)
);
```

## Delete / Drop Table

```sql
DROP TABLE tablename;
```

## Show Tables

```sql
SHOW TABLES;
```

## Insert Row / Record

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept, is_admin, register_date) values ('Brad', 'Traversy', 'brad@gmail.com', '123456','Massachusetts', 'development', 1, now());
```

## Insert Multiple Rows

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept,  is_admin, register_date) values ('Fred', 'Smith', 'fred@gmail.com', '123456', 'New York', 'design', 0, now()), ('Sara', 'Watson', 'sara@gmail.com', '123456', 'New York', 'design', 0, now()),('Will', 'Jackson', 'will@yahoo.com', '123456', 'Rhode Island', 'development', 1, now()),('Paula', 'Johnson', 'paula@yahoo.com', '123456', 'Massachusetts', 'sales', 0, now()),('Tom', 'Spears', 'tom@yahoo.com', '123456', 'Massachusetts', 'sales', 0, now());
```

## Select

```sql
SELECT * FROM users;
SELECT first_name, last_name FROM users;
```

## Where Clause

```sql
SELECT * FROM users WHERE location='Massachusetts';
SELECT * FROM users WHERE location='Massachusetts' AND dept='sales';
SELECT * FROM users WHERE is_admin = 1;
SELECT * FROM users WHERE is_admin > 0;
```

## Delete Row

```sql
DELETE FROM users WHERE id = 6;
```

## Update Row

```sql
UPDATE users SET email = 'freddy@gmail.com' WHERE id = 2;

```

## Add New Column

```sql
ALTER TABLE users ADD age VARCHAR(3);
```

## Modify Column

```sql
ALTER TABLE users MODIFY COLUMN age INT(3);
```

## Order By (Sort)

```sql
SELECT * FROM users ORDER BY last_name ASC;
SELECT * FROM users ORDER BY last_name DESC;
```

## Concatenate Columns

```sql
SELECT CONCAT(first_name, ' ', last_name) AS 'Name', dept FROM users;

```

## Select Distinct Rows

```sql
SELECT DISTINCT location FROM users;

```

## Between (Select Range)

```sql
SELECT * FROM users WHERE age BETWEEN 20 AND 25;
```

## Like (Searching)

```sql
SELECT * FROM users WHERE dept LIKE 'd%';
SELECT * FROM users WHERE dept LIKE 'dev%';
SELECT * FROM users WHERE dept LIKE '%t';
SELECT * FROM users WHERE dept LIKE '%e%';
```

## Not Like

```sql
SELECT * FROM users WHERE dept NOT LIKE 'd%';
```

## IN

```sql
SELECT * FROM users WHERE dept IN ('design', 'sales');
```

## Create & Remove Index

```sql
CREATE INDEX LIndex On users(location);
DROP INDEX LIndex ON users;
```

## New Table With Foreign Key (Posts)

```sql
CREATE TABLE posts(
id INT AUTO_INCREMENT,
   user_id INT,
   title VARCHAR(100),
   body TEXT,
   publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
   PRIMARY KEY(id),
   FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## Add Data to Posts Table

```sql
INSERT INTO posts(user_id, title, body) VALUES (1, 'Post One', 'This is post one'),(3, 'Post Two', 'This is post two'),(1, 'Post Three', 'This is post three'),(2, 'Post Four', 'This is post four'),(5, 'Post Five', 'This is post five'),(4, 'Post Six', 'This is post six'),(2, 'Post Seven', 'This is post seven'),(1, 'Post Eight', 'This is post eight'),(3, 'Post Nine', 'This is post none'),(4, 'Post Ten', 'This is post ten');
```

## INNER JOIN

```sql
SELECT
  users.first_name,
  users.last_name,
  posts.title,
  posts.publish_date
FROM users
INNER JOIN posts
ON users.id = posts.user_id
ORDER BY posts.title;
```

## New Table With 2 Foriegn Keys

```sql
CREATE TABLE comments(
	id INT AUTO_INCREMENT,
    post_id INT,
    user_id INT,
    body TEXT,
    publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY(id),
    FOREIGN KEY(user_id) references users(id),
    FOREIGN KEY(post_id) references posts(id)
);
```

## Add Data to Comments Table

```sql
INSERT INTO comments(post_id, user_id, body) VALUES (1, 3, 'This is comment one'),(2, 1, 'This is comment two'),(5, 3, 'This is comment three'),(2, 4, 'This is comment four'),(1, 2, 'This is comment five'),(3, 1, 'This is comment six'),(3, 2, 'This is comment six'),(5, 4, 'This is comment seven'),(2, 3, 'This is comment seven');
```

## Left Join

```sql
SELECT
comments.body,
posts.title
FROM comments
LEFT JOIN posts ON posts.id = comments.post_id
ORDER BY posts.title;

```

## Join Multiple Tables

```sql
SELECT
comments.body,
posts.title,
users.first_name,
users.last_name
FROM comments
INNER JOIN posts on posts.id = comments.post_id
INNER JOIN users on users.id = comments.user_id
ORDER BY posts.title;

```

## Aggregate Functions

```sql
SELECT COUNT(id) FROM users;
SELECT MAX(age) FROM users;
SELECT MIN(age) FROM users;
SELECT SUM(age) FROM users;
SELECT UCASE(first_name), LCASE(last_name) FROM users;

```

## Group By

```sql
SELECT age, COUNT(age) FROM users GROUP BY age;
SELECT age, COUNT(age) FROM users WHERE age > 20 GROUP BY age;
SELECT age, COUNT(age) FROM users GROUP BY age HAVING count(age) >=2;

```

# InfluxDB (Time Series)

- As we know the InfluxDB is the time series db.
- its written in Go.
- some of The time series data are

  1. stock trades and quotes(the summarization of the events happening in the real time)
  2. Metrics (of servers / services)
  3. Analytics (user Analytics)
  4. Event Stream
  5. Sensor Data(such as iot)

- IN InfluxDB there are 2 types of time series data

  1. Regular Time series (samples at regular intervals)
  2. Irregular Time series ( which are largely event driven ). and the thing about the irregular time series is we can induce a regular time series into a irregular one.
     - ex - if we re tracking the req res time and we want like give us the min, max and the mean of the ten minute windows for the last 24 hrs.

- Why we want the time series data / db ?

  - **Scaling** - ex in the server monitoring like 2000 of server monitoring and the vm or sensor units
  - so it will be like 1000 measurement units per server/unit every 10s and the totally 17,280,000,000 distinct points per day and we don't wanna stick that to the regular db
  - Now the **Compressing** becomes very important, and we want to
  - **Aging out the old data**
  - **DownSampling** - if we want the summaries of longer term views of data collect like min, max, sum, count automatically in different windows of time.
  - **Fast Range Queries** - to return the entire series or to do a computation on the series of events. ex - like give the 7 days worth of data and the summary of the data.

- with influx its like the 2 Databases in the one s/m
  - **RAw Time series Db** - for storing the time series data
  - **Inverted Index** - for matching the meta data to the actual time series we wanna do the computations on.
- Everything is indexed by time, and the unique series we're tracking.
- and the data is organized into **Shards** which is basically the contiguous **block of time**
- with the influx db we ve like the text based protocol to write the data into our database.
- and it is basically **Schemaless** so we don't ve to create a table and define the schema in advance.
- here we will basically create the db and tells the retention policy, and the policy we will tell like how long we want our data to keep around and then we will start writing the data using the http protocol.
- the data will look like (temperature, device=dev1, building=b1 internal=80, external=18 14422323) -> the measurement(are str), tags (are kvp), fields(internals and externals), and then finally the timestamp.
  - and we can actually store upto like the ns timestamps scale
- Each series and the field to the unique ID
  - ex - temperature, device=dev1, building=b1, #internal --> 1 (1445455, 18) - key(which is 1 - id and the time 1445455 ), value(which is(18))
  - temperature, device=dev1, building=b1, #external --> 2 (144545532, 29)
- so the important thing is we're using the key value store for this.
- **Awesome read performance** - is one of the key, so we can query in real time, and we can query and get the results back in ideally 100ms, so we can do the visualization.
- **Better compression** is also one of the key requirements.
- **write can't block the read** and the read can't block the write so we need to ve better locking s/m.
- **write multiple ranges of time simultaneously** - w/o blocking the performance of the db.
- **hot backups**
- **Many dbs open in a single process** - we want to ve multiple dbs open in a single process w/o blowing up the whole no.of file handles.
- **TSM** so for all those key features ve to supported for that they created the storage s/m called time structured merge tree. (like the LSM tree)

### components (are similar to the LSM tree)

1. Write Ahead Logs - they ve the write ahead log (WAL). which is append only file
2. IN mem cache - periodically we will flush the in memory cache onto the index files or tsm files (on disk)
3. index files - we will memery map all these files (from the in mem cache), so we can access them like an array in mem.

- **Structure of the tsm file** - Header(5bytes) - magic 4b str and 1b for the version,
- Blocks(N bytes) - within the we ve CRC(4b) and compressed data (N bytes),
- index(N bytes) looks like keylen(2b str) and key(Nbyte),Type - 1b, count -2b, mintime - 8b, maxtime - 8b, offset -8b, size - 4b.
- Footer(4 bytes) - index of 8b. the footer is just the offset of where the index is located in the file.

- **compressed data** - the compressed data will look like - Type (1 byte), len(Vbyte), Time stamp(N bytes), value (N bytes)
  - by default they will compress upto 1000 values into a single block like the above.
- TimeStamps - encoding based on the precision and the deltas. and they took the compression to each block based on the shape of the time stamp they have and the best case is (**Run length Encoding**)
- **float64** - for the float64 they uses double delta compression which is similar to the fb gorilla time series compression.
- **bool** - are bits
- **int64** - uses double delta, zig-zag same as the protobuffs.
- **str** - uses snappy

- **updates -** write resolve at query.
- **Deletes** - tombstone, resolve at query and compaction.
- **compaction** - combine multiple tsm files, put all the series points into same file.. serires points in 1k block, multiple levels (like the levelDb)

  - full compaction when cold for writes

- **Query lang** - the query language they ve developed for this is kinda looks like the sql but not sql(like mutant but they are going to impl the functional query lang) ex - `select percentile(90, value) from cpu where time > now() - 12h and "region" = 'west' group by time(10m), host`

- now how do map to series (how do we map the bits of metadata to the time series under the hood.)
- **Inverted index** - for that they use the inverted index, as we know the inverted index are good for the full text search. where we index the bunch of docs and then we match the match terms appeared in the docs to the ids of thd doc itself.

  - but here they are matching the metadata about the time series to the actual series. ex - series to ID, measurements to fields, host to vals, regions to vals, and then posting lists.

- **Index V1** - is IN - memory, load on boot, memory constraits, hence slower boot times with higher cardinality.
- **Index v2** (they were building) - in mem index, and on disk index(do we already ve), if not (WAL - append only), from there periodic flushes (on disk indices) and then later compaction.

- **Index File Layout** - series block, Tag block, Tag block N, Measurement block, offsets
  - the series block has the series key, hash index, etc.
  - the hash index - for the hash index they are using **Robin hood hashing** - which can fully load table, No linked list for lookup, perfect for read only hashes
  - the series block also has the HLL (which is a cardinality estimation) - HyperLogLog ++ . HyperLog is like a sketch (or approximation) or accounting for the bunch of strings

## Influx DB (tutorial)

- since its written in go, so it compiles into a single binary. and also it requires no external dependencies such as Hbase, cassandra, redis, zookeeper.
- the time series is the series of data points indexed in the timely ordered.

- **Time Series**
  - is a collection of data points with a certain time
  - all data points are valuable measurements within a certain time interval.
  - interval b/w the data points should be the same(saves disk)
  - each data point has only one measurement
- The regular time series the data measure points happening at regular intervals(devops monitoring like cpu, ram), and irregular the data measurements points happening at irregular intervals(user clicks, restaurant order).

- **TICK stack** is a collection of **Telegraph** (which is a event/ logs/ metric aggregator), influx db, chronologh(is visualization platform like grafana), and Kapacitor (like kafka real time streaming and data processing) it can also process both stream and the batch data from influxdb.
  - **kapacitor** - can also do operator alerts, call the user defined functions, 
  - also provides auto-scaling, stock reordering, iot control, and automate the complete s/m.
- the influxdb offers 2 features, 1.  continuous queries (CQ) and 2. Retention policy(RP)

**Key concept of influxdb** - some of the key concepts are
  - Database
  - field key, set, value 
    - **Field val** - is the actual data can be str, bool, int, float timestamps. ex - 13.45.
    - **field key** - are the str and they store the metadata ex - speed
    - **Field set** - are the combination of both ex - speed=13.43
  **-point** - is the field set with the same series in same timestamp.
  - **tag key, set, value** - tags are important in the influxdb, if we do this right then our queries will be lightning fast. and its important for the cardinality of our DS
    **- tag val** - extra metadata ex - paris, bmw and audi
    **- tag key** - extra metadata ex - location or car
    **- tag set** - is the difference combination of the tag key and the tag val ex - location=paris, car=audi
  **- series** - is the collection of data that share the retention policy, measurement and the tag set.
  - **timestamp** - since influxdb is a time series data store, every measurement does have a timestamp
    - epoch time - follows the epoch time.
  - **measurement** - act as a container for the tags, fields and time column (comparison with the table in the sql)
  - **Retention policy(RP)** - this tells how long the influx db keeps data (duration), and how many copies of those data are stored in the cluster(replication).

**Note** - don't encode the data into measurement names - ex speed_ms.jorn.france speed=29.33 instead speed_ms, driver=jorn, location=france,  - so we can see it will limit us while querying if we use the measurement name.
  - we can use the **graphite plugin** to convert the data into correct influxdb live protocol data
  - then don't overload the tags with the metadata.

**Other best practices**
  - functions only accepts fields.
  - Regex only works on tags (better to avoid em)
  - GroupBy only works on tags.
  - Don't use same name for a field and tag.
  - use enough tags 
  - when use ms (miliseconds) there is way more traffic.
    - and they are larger on disk.
    - so the write throughput may get saturated.
  - Other tips - Losing info due to wrong data types.. 10 measurements in a db ok but 100 measurements in a db is not ok.

### Influx Data model
- the x - axis is the time stamp. represented as a number in the unix epoch time.
- it uses the **line protocol**, this is how the measurements coming into the database.

- **Influx QL** - is the ql like 
- **schema exploration** queries always starts with **show** ex - SHOW DATABASES
- **Data Exploitation** - other queries like the select, from etc 
- **select data with specific timestamps** - ex - select * from speed_ms where time > now() - 150d; 
  - note: if we import the data with the wrong precision, then there is chance the query will fail.
    - to fix this he reruns the imports (from the docker) with the correct precision(-precision s). the cmd can be found in the doc.
- **GroupBy** - we can also group by the queries based on the time - ex  - we can set start time and the end time (by gt or less) and then use "Group By time(12m)" with the 12min time frame
- **INTO** - the into clause just like the sql helps to write the query results to the user specified measurement.
  - renaming the db is not possible
  - rename the smaller pieces when the Dataset is huge.
    - so for that we can run the queries with "into" and followed by different destinations(dbs), retention policy and measurements etc and then use the time boundaries in the where clause. so by doing this will prevent our s/m running out of memories.
  - we can write the result of the query to a measurement.
  - write the result of the query to a full qualified measurements (means by specifying its db and the retention policy).
  - Another common use case of the INTO clause is **Down sampling** -ex - taking the high precision data and aggregation em into lower precision and then storing the lower precision data into db.

**Mean**
  - calculate the mean difference b/w 2 fields

**Joins** 
  - as we know there are no joins in influxdb.

**Http Api writing**
- to create the db look the curl command.
- lly look the curl command for writing single point (query) and multiple points
- writing the points from the file (which will put all the data from the file into the db)

- **Http api reading**
  - lly for the reading query.
  - **chunking** - can be used to return the results in a stream batches rather than a single res. just by setting the "chunked=true" and "chunk size" options in the curl http call.
### Authentication and authorization (influx)

- by default the authentication is disabled, so all user can ve all the privileges.
- and the authorization is only available if we ve enabled authentication. in the .config file (/etc/influxdb/influxdb.config)
- its very similar to the sql authentication and authorization (for both cli and curl/ http api)
- the general user management like - Set user, remove user (drop user user_name)
- for the admin user create the user first with the password and all privilege (which is admin)
  - lly we can use Grant Read on - to give the read only access to user.
    - "Revoke All" to revoke all privileges of the user.
<h2> Retention policy </h2> 
- single db can ve several RP and the RP are unique per db.
- **Duration** - tells how long the db is going to keep data (min - 1hr, max - infinitive)
- **Replication** - this clause determines the replication factor, how many independent copies of each point are stored in a cluster.
- **Shard Duration** - determines the duration covered by the shard group (by default the shard group duration is determined by the retention policy)
- **Alter** - use the alter key to alter the retention policy.
- **drop** lly drop/ delete the retention policy

<h2> continuous queries(CQ)</h2> ex - create continuous query "cq_basic" ON "transportation" BEGIN "..." END "..." // and in the Begin clause we can use groupBy(1h) so the query will run now minus the groupby time(1h prior to now) interval.
  - inside the groupBy(1h, 15m) the second arg, 15m is the offset interval, will forces the cq to execute after the default time. 
-  is another feature, we can write the continuous queries by using "CREATE CONTINUOUS QUERY ON" db begin and end
- so it will run periodic. and this CQ operate on real time data and store query results.
- it uses the local server time stamp to group by interval and influx present time boundaries to determine when to execute and what time range to cover in a query.
- we can also use the cq to down sample data from the single field and write the measurement in the same db.
- **RESAMPLE EVERY** clause is little advanced. CQ will execute as the same interval at every interval at the resample clause
  - when the cq execs it runs the single query for the time range b/w now - FOR interval in the resample clause 
  - we can also use the Resample every clause along with the FOR clause

<h2> Chronograf </h2>

- is the monitoring and visualization tool, dashboard solution.
- also creates alerts and autmation rules. 
- run the chronograf container with the url ref to the influx db container (so it can reach the n/w of influx).
- again in the dashboard ui add the data source of our influx container, and also we can add the connection to the telegraf
- we can add the smtp server (ex our google smtp mail) and then create the rule to notify the user if he reaches the max cpu threshold
- **Grafana** - and the same goes for the grafana as well.

<h2> TICK Stack </h2>

- the setup he does is or we can do is just use the vm and then install all the TICK stack comps not as a container but as direct package/installation
- The telegraf has like 60 or more plugins so we can import metrics from influx, apache cassandra, elastic search, docker, ip table, redis. etc. to see the usage we can use ex - `telegrag usage -redis`
- The telegraf provides the agents that helps to push daat into influx.
- **Kapacitor** - we can use this to read data from influx and generate alerts. or we can think of like data enabled alert that build on top of influx db.

--------------------------------------------------------------------------------

## DAta Logging with MQTT, Node-red, influx and grafana

- Mqtt - is a message broker to receive the messages from the sensors, can take the data from all sorts of sensors.
  - for this broker we ve to install mosquitto and the client
  - then we can use the "mosquitto_sub" command to subscribe specific topic 
- Node-red - is the data bridge b/w the message broker and the db.
  - in the node red ui we can search for the "node-red-contrib-influxdb" plugin and install
  - and in the n/w pallet we can select the mqtt-in then we can subscribe to the topic (before that connect to the mqtt broker, also set the authentication for the mqtt broker)
  - and lly we can establish the connection b/w the influx db as well
  - for the json/ chunk of data (instead of single val), then  sub to the topic(sensor), we can select the json(parser pallet) and make a connection to the topic to the parser. (so now this json will be parsed into node-red obj)
  - we can also split the vals in the json and then store em into the influxdb
- in that ex - he uses raspberry pie (ubuntu) in the sd card option and then use/install all the above 4 components

### InfluxDB with python

- using python is one of the best ways to inject data into InfluxDB.
- `pip install influxdb`
- by using the influxdb client ex `from influxdb import influxdb client` and then we can ve to use the datetime module.
- refer the influx db client python lib.. 
- we can set up the json payload (almost like the schema, since in influx we can't model our schema), we can add details like measurements(table), tags etc..
- to send / insert our payload ex `client.write_points(json_payload)`

### InfluxQL 2.0

- in the influx admin ui, we  can see the naming convention has changed, **Buckets** - are dbs.
- The Continuous Query has been renamed to **Tasks**
- and these buckets will be created based on the RetentionPolicy.
- in this newer version we can pretty much do everything in the dashboard/ui of the influx. such as write the query, visualizing, add filters etc
- **DownSampling** - the way it works, we wanna down sampling the data into one bucket and put it into another bucket. 
  - mostly the down sampling data are the aggregate or mean values
- **Note** Refer the "Influx Data/community-templates" repo for some predefined yml templates files for the queries such as for down sampling, and we can also find some eg for grafana etc.
  - [link to the is here](https://github.com/influxdata/community-templates)
