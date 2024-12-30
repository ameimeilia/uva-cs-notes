---
layout: default
title: Data 2
parent: Final Notes
nav_order: 4
---
# Databases and SQL
- **Database**: A place to store persistent data for future access by programs.
### Relational Database
- Stores data in structured tables, with relationships established via foreign keys
- Data is explicitly formatted and there are constraints on the contents of the tables to store data integrity
### Non-relationship database
- Known as **NoSQL** databases, they store data in documents, often JSON-like, without enforced structure.
## Tables
- Organizes data into **attributes** (columns) and **records** (rows)
- each attribute has a specific datatype
### DataTypes
- **Text**: UTF-8 string (like Java `String`), dynamically sized
- **Integer**: Whole number (up to 8 bytes, similar to Java `long`)
- **Real**: Floating-point number (stored in 8 bytes, like Java `double`)
- **Blob**: (Binary Large OBject) Raw data, stored as input (useful for images, files)
- **Null**: Default type allowed in any column, indicating missing or blank data
### Primary Key
- Identifies unique records in a table
	- Must be unique (no duplicates)
	- Not null (cannot be blank)
- Generally an integer, often auto-generated
# SQLite
- Library providing a self-contained **embedded** SQL database
## Embedded Database
- Database tightly integrated with the application, vs database being a separate application/server
- SQLite is invoked directly, stored as literal files on the disk
### Usages of SQLite
- **Local Storage**: Common for storing settings/data in desktop/mobile apps (e.g., web browsers).
- **Supported Platforms**: Included in Android, iOS, Mac OS X (10.4+), and Windows 10+.
- **Portability**: SQLite files are cross-platform compatible, simplifying data sharing.
### Limitations of SQLite
- **Scalability**: Not suited for high-traffic, concurrent write operations.
- **Concurrency**: Allows multiple readers but restricts to one writer at a time for data consistency.
- **Data Size**: Can store up to 281 TB, but file system limitations often impose smaller storage caps.
## sqlite3 command line
### SQLite Installation
- [install sqlite3](https://sqlite.org/download.html) and add the sqlite3 command to your system PATH variable - [a full guide for Windows and Mac OS X can be found on TutorialsPoint here](https://www.tutorialspoint.com/sqlite/sqlite_installation.htm)
### Shell Commands
- **`sqlite3 [filename]`**: Open or create a database file if no database file exists
	- will then enter **SQLite Shell**
	- if used with with no filename, the SQLite shell will open with an **in-memory database**
- **`.help`**: View all available SQLite shell commands.
- **`.open [file]`**: Open a new database file within the SQLite shell.
- **`.quit`**: Exit the SQLite shell.
- **`.tables`**: List tables in the current database.
- **`.dump`**: Generate an SQL script of the current database state.
	- often used with the `.output` command to dump the contents to a file, and the `.read` command to run the dumped out .sql file as a script
- **`.output`**: Redirect output to a file.
#### `.output`

By default, sqlite displays writes all contents to the console. For instance:

```sqlite
sqlite> .output myFile.txt
--all commands after .output will be output to the same file
sqlite> .help
slqite> .quit
--to stop writing to a file and write to console again:
sqlite> .output dump.sql    --overwrites any existing file
sqlite> .dump    -- generates an sql script that will recreate all database data
sqlite> .output
```

## SQLite quirks
- **No User Access Control**: no access control permissions, no “database user,” files are not encrypted by default
- **Flexible Typing**: SQLite allows storing various data types in columns without strict enforcement and does not enforce maximum lengths for datatypes
- **Nullable Primary Keys**: Permits one nullable primary key due to legacy compatibility.
- **Foreign Keys**: Disabled by default; must enable with `PRAGMA foreign_keys = ON`.
- **Type Mismatches**: `1 = '1'` is false; differs from other SQL systems where this would be true.
- **Missing Boolean/DateTime Types**: Uses integer representation for Boolean (1 for TRUE, 0 for FALSE) and Unix time for dates
# Basic SQL Queries
## SQL Queries
- **CRUD Operations**: SQL commands to Create, Read, Update, and Delete data in database tables.
- **Core SQL Commands**:
    - `CREATE`: Create a new table structure.
    - `INSERT`: Add new records to a table.
    - `SELECT`: Retrieve data from one or more tables.
    - `UPDATE`: Modify existing data in a table.
    - `DELETE`: Remove records from a table.
    - `DROP`: Remove an entire table, including its data and structure.
- **Transaction management**: Controls the commit of changes to maintain data integrity.
	- `BEGIN TRANSACTION`: Start a transaction; changes are temporary until committed.
	- `COMMIT`: Make all changes in the transaction permanent.
	- `ROLLBACK`: Undo changes since the last `BEGIN TRANSACTION`.
- **Auto-Commit Mode**: SQLite commits all changes by default; transactions disable auto-commit until completed.
### Case-sensitivity
- SQL commands and table/column names are **case-insensitive**
- text literals are case-sensitive
### Create Table
#### Syntax

```sqlite
CREATE TABLE [IF NOT EXISTS] table_name    -- tables are required to be unique
(
	-- datatypes are required when using STRICT tables
    column_name1 data_type1 [column_constraints1],    
    column_name2 data_type2 [column_constraints2],
    -- as many columns as needed
    [table_constraints]
) [STRICT];

-- to see the table
sqlite> .tables
[table_name]

-- to see the structure and constraints of the table again
sqlite> .schema [table_name]
```

#### Column Constraints
*example - table creation*

```sqlite
CREATE TABLE Books (
    BookID INTEGER PRIMARY KEY,
    Title TEXT NOT NULL,
    Series TEXT,
    SeriesOrder INTEGER,
    Author TEXT NOT NULL,
    Published INTEGER NOT NULL,
    Rating REAL DEFAULT 1.0 CHECK(Rating >= 1.0 AND RATING <= 5.0)
) STRICT;
```

- `PRIMARY KEY`: make a column the primary key; should be first column and integer; entries must be **Unique** and **Not Null**
- `NOT NULL`: requires that an attribute **must** be present
- `UNIQUE`: **no two records** can have the same value in this column.
- `DEFAULT`: if column is `null` when inserting data, that `null` value is replaced by the default value
- `CHECK`: includes a **boolean expression**; when inserted, if this expression returns FALSE, the inserted record is rejected and an error message given
#### Table Constraints
*example - unique (Title, Author) pair*

```sqlite
CREATE TABLE IF NOT EXISTS Books (
  BookID INTEGER PRIMARY KEY,
  Title TEXT NOT NULL,
  Series TEXT,
  SeriesOrder INTEGER,
  Author TEXT NOT NULL,
  Published INTEGER NOT NULL,
  Rating REAL DEFAULT 1.0 CHECK(Rating >= 1.0 AND RATING <= 5.0),
  -- add table constraint, defined over 1 or more columns
  UNIQUE(Title, Author)
) STRICT;
```

#### Changing Tables after creation
1. **Create a New Table**: Name it differently but include existing columns plus any modifications.
2. **Copy Data**: Use `INSERT` to transfer data from the old table to the new table.
3. **Drop the Old Table**: Use `DROP` to delete the outdated table.
4. **Rename the New Table**: Use `ALTER TABLE` to change the name of the new table to the original name.

- **Avoid Frequent Alterations**: Plan table design carefully to reduce the need for alterations.
- **Backup Databases**: Always backup before making any structural changes to avoid data loss.
### INSERT

```sqlite
-- add one record into table Books
-- NEVER blind insert (insert without column names)
INSERT INTO Books(BookID, Title, Series, SeriesOrder, Author, Published, Rating)
  VALUES (1, 'Hitchhiker''s Guide to the Galaxy', 'Hitchhiker''s Guide to the Galaxy',
         1, 'Douglas Adams', 1979, 4.2);
-- double '' to indicate apostrophe, not end of string

-- add multple rows in one query
INSERT INTO Books(Title, Series, SeriesOrder, Author, Published, Rating)
    VALUES ('Memories of Ice', 'Malazan Book of the Fallen',3, 'Steven Erikson', 2001, 5.0),
           ('Unsouled', 'Cradle', '1', 'Will Wight', 2016, 3.7);

-- to insert NULL values, simply ignore the column when inserting
INSERT INTO Books(Title, Author, Published, Rating)
  VALUES ('Happier as Werewolves', 'Andrew Heaton', 2016, 3.9);

-- to check INSERT use SELECT
sqlite> SELECT * FROM Books;
1|Hitchhiker''s Guide to the Galaxy|Hitchhiker''s Guide to the Galaxy|1|Douglas Adams|1979|4.2
2|Memories of Ice|Malazan Book of the Fallen|3|Steven Erikson|2001|5.0
3|Unsouled|Cradle|1|Will Wight|2016|3.7
4|The Shadow Rising|The Wheel of Time|4|Robert Jordan|1992|4.8
5|Happier as Werewolves|||Andrew Heaton|2016|3.9
-- BookID is auto-incremented since it is an INTEGER PRIMARY KEY
```

- data is **persistent** and can only be removed using `DELETE` or `DROP` command
### SELECT

```sqlite
-- to display the entire table
sqlite> SELECT * FROM Books;

-- to display only the specified columns from the (optional) specified table
-- specify table when querying over multiple tables
sqlite> SELECT Books.Title, Books.Author FROM Books;
Title                             Author
--------------------------------  --------------
Hitchhiker''s Guide to the Galaxy  Douglas Adams
Memories of Ice                   Steven Erikson
Unsouled                          Will Wight
The Shadow Rising                 Robert Jordan
Happier as Werewolves             Andrew Heaton

-- use WHERE [boolean_expression]
sqlite> SELECT Title, Author, Published, Rating
    FROM Books
    WHERE Rating >= 4.0;
   
Title                             Author          Published  Rating
--------------------------------  --------------  ---------  ------
Hitchhiker''s Guide to the Galaxy Douglas Adams   1979       4.2
Memories of Ice                   Steven Erikson  2001       5.0
The Shadow Rising                 Robert Jordan   1992       4.8

-- use LIKE to filter by Text
-- case insensitive
SELECT Title, Author
   FROM Books
   WHERE Title LIKE 'THE SHADOW RISING';

Title              Author
-----------------  -------------
The Shadow Rising  Robert Jordan

-- every title that contains LIKE "THE" at any position
SELECT Title, Author
    FROM Books
    WHERE Title LIKE "%the%";

Title                               Author
----------------------------------  -------------
Hitchhiker''s Guide to the Galaxy   Douglas Adams
The Shadow Rising                   Robert Jordan
The Long Dark Tea-Time of the Soul  Douglas Adams

-- use ORDER BY to define a sorting order
SELECT Title, Author, Rating
    FROM Books
    ORDER BY Rating DESC;    -- ASC (ascending by default)

Title                               Author          Rating
----------------------------------  --------------  ------
Memories of Ice                     Steven Erikson  5.0
The Shadow Rising                   Robert Jordan   4.8
Hitchhiker''s Guide to the Galaxy   Douglas Adams   4.2
Happier as Werewolves               Andrew Heaton   3.9
Unsouled                            Will Wight      3.7
The Long Dark Tea-Time of the Soul  Douglas Adams   2.3

-- to sort alphabetically in a case-insentive way
-- remember that Strings are sorted by character Unicode values
SELECT Title, Author, Rating
    FROM Books
    ORDER BY LOWER(Title);

-- sort by multiple criteria
sqlite> INSERT INTO BOOKS (Title, Series, SeriesOrder, Author, Published, Rating)
    VALUES ("Deadhouse Gates", "Malazan Book of the Fallen", 2, "Steven Erikson", 2000, 4.9);

-- sort by Series alphabetically then sort by SeriesOrder in ASC order
SELECT Title, Series, SeriesOrder
   FROM Books
   WHERE Series is not NULL
   ORDER BY 
       LOWER(Series),  
       SeriesOrder;  --remember: ASC by default

Title                               Series                            SeriesOrder
----------------------------------  --------------------------------  -----------
Unsouled                            Cradle                            1            
The Long Dark Tea-Time of the Soul  Dirk Gentley                      2            
Hitchhiker's Guide to the Galaxy    Hitchhiker's Guide to the Galaxy  1            
Deadhouse Gates                     Malazan Book of the Fallen        2            
Memories of Ice                     Malazan Book of the Fallen        3            
The Shadow Rising                   The Wheel of Time                 4            
```

### Transactions
- In SQLite, by default **auto-commit is on**!
- before **ever** using `UPDATE` or `DELETE`, do the following:
	1. **Make a back-up**: copy the database file and paste it somewhere else
	2. `BEGIN TRANSACTION`
#### `BEGIN TRANSACTION`, `ROLLBACK`, and `COMMIT`
- `BEGIN TRANSACTION` sets a “restore point”

```sqlite
sqlite> BEGIN TRANSACTION;
sqlite> DELETE FROM Books; --oops I accidently put a semicolon when I only wanted to delete a specific book
sqlite> SELECT * FROM Books;
```

- `DELETE FROM Books` is currently **transient**, a pending change that hasn’t been saved permanently
- use `ROLLBACK` to restore to the state at `BEGIN TRANSACTION`

```sqlite
sqlite> ROLLBACK;
```

- use `COMMIT` to permanently store the changes in the database
### UPDATE
- used to _change_ existing data

```sqlite
BEGIN TRANSACTION;

-- where clause is VERY IMPORTANT to ensure only desired data is changed
UPDATE Books
    SET Author = "Steve Rune Lundin"
    WHERE Author = "Steven Erikson";

-- check that UPDATE was used correctly
SELECT * FROM Books;
```

### DELETE
- general rule: when removing a **single specific record** use its Primary Key

```sqlite
-- to identify the BookID
SELECT ID, Title, Author
    FROM Books
    WHERE Author = "Will Wight"
    AND Title = "Unsouled";

BookID  Title                               Author             
------  ----------------------------------  ----------
3       Unsouled                            Will Wight

BEGIN TRANSACTION; -- ALWAYS ALWAYS ALWAYS!!!

DELETE FROM Books
    WHERE BookID = 3;

-- check that DELETE was used correctly
SELECT * FROM Books;

-- to "truncate" or delete all data without deleting table schema:
DELETE FROM Books;
```

### DROP
- deletes the entire table, including schema

```sqlite
-- throws error if table doesn't exist
DROP TABLE Books;

DROP TABLE IF EXISTS Books;
```

### UPSERT
- combination of insert and update
- use unique ID to determine whether query is an `INSERT` or `UPDATE`
- danger: if wrong ID is used, record will be corrupted or replaced

```sqlite
INSERT INTO Books(BookID, Title, Series, SeriesOrder, Author, Published, Rating)
    VALUES (6, 'The Long Dark Tea-Time of the Soul', 'Dirk Gently Holistic Detective Agency', 2, 'Douglas Adams', 1988, 2.7),
           (null, 'Dirk Gently''s Holistic Detective Agency', 'Dirk Gently', 1, 'Douglas Adams', 1987, 3.1)
    ON CONFLICT (BookID) DO UPDATE 
        SET Rating = excluded.Rating;

-- ON CONFLICT is similar to using a try-catch block
-- excluded is the row that caused a conflict

sqlite> Select BookID, Title, Rating from Books;
BookID  Title                                    Rating
------  ---------------------------------------  ------
1       Hitchhiker''s Guide to the Galaxy        4.2
2       Memories of Ice                          5.0
3       Unsouled                                 3.7
4       The Shadow Rising                        4.8
5       Happier as Werewolves                    3.9
6       The Long Dark Tea-Time of the Soul       2.7    -- rating updated
7       Dirk Gently''s Holistic Detective Agency 3.1    -- record added
```

### Ensuring unique ID
- besides inserting `null`, get the existing max ID then add one to it

```sqlite
SELECT MAX(BookID)+1 FROM Books; 
```