# Relational Database and SQL for Web Programming

## 1.Use Database to Store Data
Database store data in a **table** with rows and columns.

SQL is a language used to interact with table.

## 2.Type of Data
* Integer
* Decimal
* Serial -> used for data that should automatically increase when adding new items
* Varchar -> text 
* Timestamp -> store time 
* Boolean
* Enum -> one of a finite number of discrete possible values -> like JAVA

## 3.CREATE
**(Create a table)**

In PostgreSQL, use the following commands to set up:
```
psal fileName (or url)

CREATE TABLE flights(........);

\d (Used to check all tables existing in current database)
```

Use the following syntax to create a table storing info of flights
```
CREATE TABLE flights(
	id SERIAL PRIMARY KEY; #means every flight has a unique id
	origin VARCHAR NOT NULL; #means that every flight must have    
                           #an origin
	destination VARCHAR NOT NULL;
	duration INTEGER NOT NILL;
);
```


## 4.Constrains
**(property that a record may have to have)**
* NOT NULL
* UNIQUE  (every item has a unique value)
* PRIMARY KEY (the entrance you access a item)
* DEFAULT (when most item has such a default value)
* CHECK ( for example,only allow items less than or larger than)

## 5.INSERT
**(Insert Record to Table)**

You need specify necessary value (and their type) of a new record, except for default, serial these automatically created value:
```
INSERT INFO flights (origin,destination, duration) VALUES(‘New York’, “London”, 415);

# NO need to set id, cause is automatically increasing as entry 
#increase
```

## 6.SELECT
**(read properties of a record in a table)**

```
SELECT * FROM flights; # select all properties of a record
SELECT origin, destination FROM flights;
SELECT origin, destination FROM flights WHERE id= 3;
SELECT origin, destination FROM flights WHERE origin=‘London’;
SELECT origin, destination FROM flights WHERE origin in (‘London,’New York’);
SELECT origin, destination FROM flights WHERE duration>300;

# Use Boolean Calculation
SELECT origin, destination FROM flights WHERE origin=‘London’ AND duration>300;
SELECT origin, destination FROM flights WHERE origin=‘London’ OR duration>300;

# Use functions
SELECT AVG(duration) FROM flight;
SELECT AVG(duration) FROM flight WHERE destination=‘LONDON’;
SELECT MAX(duration) FROM flight;
SELECT MIN(duration) FROM flight;
SELECT SUM(duration) FROM flight;
SELECT COUNT(*) FROM flights; #count the number of entrance;
SELECT COUNT(*) FROM flights WHERE origin=‘MOSCOW’; #count the number of entrance with special property;
SELECT COUNT(*) FROM flights WHERE origin=‘MOSCOW’;

# Use Regular Expression
SELECT * FROM flight WHERE origin LIKE ‘%a%’;

# List the selected entrance in some order
SELECT duration FROM flights ORDERED BY duration ASC;
SELECT duration FROM flights ORDERED BY duration DSC;
SELECT duration FROM flights ORDERED BY duration DSC LIMIT 3;
(Only Display 3 records from the result)
```

## 7.UPDATE
(Revise data in the table)

```
UPDATE flights
	SET duration = 430
	WHERE origin = ‘New York’
	AND destination = ‘London’
```

## 8.DELETE

```
DELETE FROM flights WHERE destination = ‘London’
```

## 9.Combine Queries into a single Query
For example:
```
SELECT origin, COUNT(*) FROM flights GROUP BY origin
SELECT origin, COUNT(*) FROM flights GROUP BY origin HAVING COUNT(*) > 1
```

You will get a feed back as ![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/%E7%85%A7%E7%89%87%202018%E5%B9%B48%E6%9C%888%E6%97%A5%20%E4%B8%8B%E5%8D%8841337.jpg)


## 10.Nested Queries
SELECT will return a table.

The first layer of query:
![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/IMG_0038.JPG)

The second layer of query: using keyword `SELECT column_a, ... FROM ... WHERE column_a IN table_name `

![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/IMG_0039.JPG)

## 11.Foreign Keys
Foreign key is used to build connection between multiple tables, to make keeping track of specially connected data (each table) more easily. 

For instance: when comes to the organizing of records (like **modifying passenger’s flight**), tables of “passenger~flight_id” + “flight_id~origin~destination”, are more effective and expressive
 than a mere table “passenger~origin~destination”. 

Then when I want to modify a passenger’s flight info, like Bob’s, I will only need to modify the flight_id of Bob)

Here is a concrete example:

Table “flight” (with columns “name” “orgin” “origin_code” “destination” “destination_code”, “duration”):
	![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/%E7%85%A7%E7%89%87%202018%E5%B9%B48%E6%9C%8810%E6%97%A5%20%E4%B8%8B%E5%8D%88105918.jpg)
Since there are records like 1 and 4 existing redundancy (New York ,JFK repeats twice), we can break the Table “flight” into 3 different  tables with out too much redundancy, and keep track of more individual objects:

Record of Locations:
( id : code : name)
![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/%E7%85%A7%E7%89%87%202018%E5%B9%B48%E6%9C%8810%E6%97%A5%20%E4%B8%8B%E5%8D%88110344.jpg)

Record of Locations:
( id : origin_id : destination_id : duration )
;0![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/%E7%85%A7%E7%89%87%202018%E5%B9%B48%E6%9C%8810%E6%97%A5%20%E4%B8%8B%E5%8D%88110424.jpg)
(Use foreign key “id” to connect  tables an reduce redundancy in storage) 

Records of passenger:
( id : name : flight_id )
![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/%E7%85%A7%E7%89%87%202018%E5%B9%B48%E6%9C%8810%E6%97%A5%20%E4%B8%8B%E5%8D%88110731.jpg).             
* Grammar for Creating Tables Containing Foreign Key: 
**REFERENCES table_name**
```
CREATE TABLE passenger(
	id SERIAL PRIMARY KEY,
	flight_id INTEGER REFERENCES flight
	)
```

* Grammar for Querying Foreign Records in another referenced table:

1. SELECT column_a, column_b, column1, column2 FROM table_a **JOIN** table_b **ON** table_a.column_a = table_1.column_1 ;

This operation will only SELECT records that has a reference relationship (satisfying `table_a.column_a = table_1.column_1`).

Like:
```
SELECT origin, destination, name FROM flights JOIN passengers ON passengers.flight_id = flights.id;
```
The selected records won’t consist of flights which was not taken by any passenger in `passengers`.

**Note1**:  Here `flights.id` is the **primary key**(unique in table “flights”) of table `flight`. This is general for all foreign keys, which means that the referenced column should be **primary** or **unique** in the referenced table to make the reference explicit, or not ambiguous.

**Note2**: Once a record is being referenced from another table, this record can’t be deleted.

2. SELECT column_a, column_b, column1, column2 **FROM** table_a **LEFT JOIN** table_b **ON** table_a.column_a = table_1.column_1 ;

This operation will SELECT both the records that have a reference relationship, or records that exists in `table_a` but were not referenced by any records in `table_b`

Like:
```
SELECT name, origin, destination FROM flights LEFT JOIN passengers ON flight.id = passengers.flight_id;
```
The selected record will consists of flights.origin and flights.destination that doesn’t belongs to any passenger.

![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/IMG_0036.JPG)

## 12.CREATE INDEX
Indexes are used to **retrieve data faster.** The user cannot see the indexes, they are just used for speed up queries/ searches.

Create Index on specifics columns can speed up searching by frequently referenced columns (no all columns), when the database gets larger.

* Syntax:
```
CREATE INDEX index_name ON table_name (column1, column2, column3)
```

* Example:
```
CREATE INDEX idx_lastname ON Persons(LastName);
```
The idx_lastname will make querying from Persons by colume LastName faster.

* DROP INDEX: syntax depends on different servers.

## 13. SQL Security - SQL Injection
If your website authenticate a registered user like:
```
userName = getRequestString(“username”);
userPass = getRequestString(“password”);

sql= ‘SELECT * FROM Users WHERE Name=“ ‘ + userName + ’ ” And 	Pass=“ ‘ + userPass + ’ ”’
```

A hacker can set the password as `xxx’ OR ‘1’=‘1`	


## 14. SQL Security - Race Conditions <- SQL Transaction

For single user’s **sequence request**, everything works well. 

Assume Bank check whether your balance is more than 100$ before 
`SELECT ...`
 
![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/IMG_0041.JPG)



When 2 users use the same card at 2 ATM access the database at the same time, balance may become minus:
![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/IMG_0042.JPG)

Syntax for the SQL Transaction:
```
BEGIN
...
COMMIT
```

## 15. SQLAlchemy (SQL <-> Python)
(A Python Library that allow Python to write SQL codes)

* Set Environment
```
import os
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker

engine = create_engine(os.getenv(“DATABASE_URL”))
# Engine is an object that manage communication from Python to
# the database, like send request to database.

# getenv(“DATABASE_URL”) is creating an “environment variable” 
#(like ‘set FLASK_APP=application.py, you should set 
# DATABASE_URL as environmental variable)

# DATABASEURL can be either local or online

db = scoped_session(sessionmaker(bing=engine))  
# create scope session, used for creating different sessions 
# for different users, in order to ensure different user’s 
# stuff are separated.
```

* Send sql request 
(and get sql feedback)
```
db.execute(“SELECT origin, destination, duration FROM flights”).fetchall()
# execute() send sql request

# fetchall() fetch all sql feedbacks and store them in db
# as a list of all individual rows (as dictionariesf).
```

* Import Records to Database 
	* CSV file: Comma Separated Value (Can be opened with Office Excelpr)
![](Lecture%203%20Relational%20Database%20and%20SQL%20for%20Web%20Programming/IMG_0043.JPG)
```
import csv
def main():
    f = open("flights.csv")
    reader = csv.reader(f)
    for origin, destination, duration in reader:
        db.execute("INSERT INTO flights (origin, destination, duration) VALUES (:origin, :destination, :duration)",
                    {"origin": origin, "destination": destination, "duration": duration})
	      # “:origin”,”:destination”,”:duration” are placeholders
        # using python dictionary to tell placeholder what to 
        # take in (now variables can show up I n dictionary)
        print(f"Added flight from {origin} to {destination} lasting {duration} minutes.")
    db.commit()
	  # No change was made to db, until db.commit is run.
    # Commit changes to database, for security purpose.
```
 
## 16. SQL <-> Flask / Database <-> Web Programming
Data Flow: SQL server -> local Python code (on Web Server) -> HTML(website) built from HTML template, and dat
