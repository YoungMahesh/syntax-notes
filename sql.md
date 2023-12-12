# sql commands

```sql
--------------------- database (mysql, postgresql) -------------------------
-- sqlite does not support this, .db file of sqlite represents a single database
CREATE DATABASE Databasename;  
SHOW DATABASES;
USE Databasename;
----------------------- table ------------------------
SHOW TABLES;
-- CREATE TABLE table (field1 type1, field2 type2);
CREATE TABLE raffle (orderId VARCHAR(255) amount(255));
CREATE TABLE sponsors (id integer PRIMARY KEY AUTOINCREMENT NOT NULL);  -- create table with column-id in libsql

TRUNCATE TABLE <table-name>;  -- delete all records from Table
DROP TABLE table_name

---------------------- columns ------------------------------
-- ALTER TABLE table_name ADD COLUMN new_column_name data_type;
ALTER TABLE sponsors ADD name text;
ALTER TABLE sponsors ADD created_at text DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE wallets ADD COLUMN sponser_id INTEGER DEFAULT 1;
ALTER TABLE table_name DROP COLUMN column_name; -- delete column
PRAGMA table_info(table_name);  -- formatted schema of table
ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name; -- rename column
-- ALTER TABLE table_name ADD column_name datatype;
ALTER TABLE table_name ADD UNIQUE (column_name_1, column_name_2); -- combination of two values should be unique

------------------------ read rows --------------------------------------
SELECT * FROM table;  -- see contents of table
SELECT * FROM tablename1 LIMIT 10;  -- check content from all columns of table (max 10)
SELECT column1, column2 FROM tablename1 LIMIT 10;  -- check content from column1,column2 of table (max 10)
SELECT COUNT(*) FROM tablename1;  -- count number of rows in table

------------------------ write rows --------------------------------------
--INSERT INTO table1 (field1, field2) VALUES (value1, value2);
INSERT INTO sponsors (name) VALUES ('jonah');

-- delete rows from table `orders` where id is 3 or 4 or 5 or 6
DELETE FROM orders WHERE id IN (3,4,5,6);

-- UPDATE table-name SET insert_into_column = from_column
UPDATE wallets SET column2 = column1 -- copy data from column1 to column2 in wallets-table
```

## foreign key

```sql
-- in sqlite you cannot add foreign_key on existing column, you have either i) define it during creation of table or ii) create a new column
ALTER TABLE wallets ADD COLUMN sp_id INTEGER REFERENCES sponsors(id);
-- in this example, column sp_id will be created with all rows with value = NULL
```

- creates a relationship between the tables based on the values of the foreign key column(s) and the primary key column(s) in the referenced table. This relationship is often referred to as a "foreign key constraint."

  1. Primary Key (PK): A primary key is a unique identifier for a record in a table. It ensures that each record can be uniquely identified. A table can have only one primary key.

  2. Foreign Key (FK): A foreign key is a column or a set of columns in a table that refers to the primary key of another table. It establishes a link between the two tables.

- When you define a foreign key in a table, you are essentially creating a relationship between the table with the foreign key and the table with the primary key.

  - This relationship helps maintain referential integrity, ensuring that the values in the foreign key column(s) correspond to existing values in the primary key column(s) of the referenced table.

- Here's an example:

  ```sql
  CREATE TABLE Employees (
      EmployeeID INT PRIMARY KEY,
      EmployeeName VARCHAR(50),
      DepartmentID INT,
      FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
  );

  CREATE TABLE Departments (
      DepartmentID INT PRIMARY KEY,
      DepartmentName VARCHAR(50)
  );
  ```

  - In this example, the `Employees` table has a foreign key (`DepartmentID`) that references the primary key (`DepartmentID`) of the `Departments` table. This establishes a relationship between the two tables, indicating that the `DepartmentID` values in the `Employees` table must correspond to existing `DepartmentID` values in the `Departments` table.

# sqlite

```bash
# tables
sqlite3 <path of .db file> # open sqlite3-cli connected with a database in .db file
# you can execute sql-commands in sqlite3-cli to view/update data
.tables  # list all tables in database
.database # list currently connected databases
.exit
.shell pwd # print present working directory

# install on ubuntu
sudo apt install sqlite3
sqlite3 --version
```

### turso

- turso used libSql (a fork of sqlite) as it's database

```bash
# https://docs.turso.tech/reference/turso-cli
turso --version
turso auth login
turso auth token
turso db tokens create <db-name>  # create auth-token for database
turso db list  # list turso databases
turso db show <db-name> # get database information
turso db shell <db-name> # get access to database shell to execute sql-queries 
```


# postgresql

- run postgresql with docker container: https://hub.docker.com/_/postgres

```bash
#------------------- docker container configuration for these docs -------------------
# container_name: postgres
# user: postgres

#--------------------- outside postgres-docker-container -----------------------------------
docker exec -it postgres bash  # go inside postgresql container, 'postgres' is container-name
docker cp postgres:/backups/backup-name.dump .  # copy dump-file from container to outside container
docker cp ./backups postgres:/backups/   # copy data from outside container to inside containe

#--------------------- inside postgres-docker-container ----------------------------------------
# backup and restore postgresql
# https://www.postgresql.org/docs/15/app-pgdump.html
mkdir backups && cd backups  # create folder to store backups
pg_dump -Fc db-name > db.dump # pg_dump is pre-installed on postgres docker-image
# -Fc => -F=format, c=custom
pg_restore -x -U postgres -d mydb /mydb.dump  # -U postgres == user named `postgres` will be the owner of database
# -d mydb /mydb.dump   ==  restore dump file in database named `mydb`
# -x == --no-privileges, avoids errors due to differennt in database_owner @backup_container and @restore_container 
pg_restore -x -d 'postgres://user1:xyz@example.com/timers' timers_5nov.dump # restore to remote_database

# ----------------- inside postgresql of postgres-docker-container -------------------------------
psql -U postgres # connect to sql-cli of container with user `postgres`
# restore database from text dump file
# if database (here `mydb`) does not exist, create it before restoring dump file
psql -U postgres -d mydb -f mydb.dump
# -U specifies the username of the PostgreSQL user that owns the database.
# -d specifies the name of the database to restore the dump file to.
# -f specifies the path to the dump file.
psql \du  # list users/roles in current-postgres

# connect with remote database
psql -h containers-us-west-96.railway.app -p 6447 -U postgres -d database1 --set=sslmode=require
# -h : The hostname of the remote PostgreSQL server.
# -p : The port number of the remote PostgreSQL server.
# -U : The username that you want to use to connect to the remote PostgreSQL server.
# -d : database name 
# --set=sslmode=require: optional, need to used when remote_database requires ssl-connection
# you will get prompt for password

```

```sql
# -------------------- sql cli of postgresql docker contaienr ------------------------------------
SELECT version();
\x on  /* turn on expanded display to view database data in better format than default */
\l  /* list databases */
create database notes1;  /* create empty database `notes1` */
drop database "<database_name>";  /* delete database */
\q  /* quit */
\c notes1;  /* connect to database named `notes1` */

/* after connected to a specific database inside sql-cli */
\dt /* list tables in database */
SELECT * FROM <table_name>;   /* view table data */
SELECT * FROM "UserCred";
/* remeber to wrap table name with string-quotes, without it UserCred will be parsed as usercred */

/* update value of column `totalPrice` to 15, at row with `id` is equal to 4, in `Cart` table */
SELECT "Cart"
SET "totalPrice" = 15
WHERE "id" = 4;
```


### backup and restore
- sometimes restore using defaults commands can give issues when owner-name, external schemas(schemas not related to database) are different
- in this case, better way is to backup and restore only tables instead of whole is database using pgAdmin
- backup data using pgAdmin
  1. click _backup_ form right-click of `public` from `Object Explorer -> Servers -> server-name -> Databases -> Schemas -> public`
  2. provide file-name
  3. click `Backup`
- restore data using pgAdmin
  1. click _restore_ from right-click of `public` of `Object Explorer -> Servers -> server-name -> Databases -> Schemas -> public`
  2. from the restore-popup-table: 1) select backup-file  2) enable `Data Options -> Do not save -> Owner`
  3. Click `Restore`


# mysql

### phpmyadmin docker container
- increase phpmyadmin file upload limit:
  - add this to env: `UPLOAD_LIMIT=500M`, here `M == MegaBytes`