- for sql commands checkout [sql](./sql.md)

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
