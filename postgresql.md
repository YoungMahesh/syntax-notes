- run postgresql with docker container: https://hub.docker.com/_/postgres

```bash
#------------------- docker container configuration for these docs -------------------
# container_name: postgres
# user: postgres

#--------------------- outside postgresql docker container -----------------------------------
docker exec -it postgres bash  # go inside postgresql container, 'postgres' is container-name
docker cp postgres:/backups/backup-name.dump .  # copy dump-file from container to outside container
docker cp ./backups postgres:/backups/   # copy data from outside container to inside containe

#--------------------- inside postgresql docker container ----------------------------------------
# backup postgresql
mkdir backups && cd backups  # create folder to store backups
pg_dump <postgres-url> > backup-name.dump  # pg_dump is pre-installed on postgres docker-image
psql -U postgres # connect to sql-cli of container with user `postgres`

# restore database from binary dump file
pg_restore -U postgres -d mydb /mydb.dump  # -U postgres == user named `postgres` will be the owner of database
# -d mydb /mydb.dump   ==  restore dump file in database named `mydb`

# restore database from text dump file
# if database (here `mydb`) does not exist, create it before restoring dump file
psql -U postgres -d mydb -f mydb.dump
# -U specifies the username of the PostgreSQL user that owns the database.
# -d specifies the name of the database to restore the dump file to.
# -f specifies the path to the dump file.


# connect with remote database
psql -h containers-us-west-96.railway.app -p 6447 -U postgres
# -h : The hostname of the remote PostgreSQL server.
# -p : The port number of the remote PostgreSQL server.
# -U : The username that you want to use to connect to the remote PostgreSQL server.
# you will get prompt for password

```

```sql
# -------------------- sql cli of postgresql docker contaienr ------------------------------------
\l  /* list databases */
create database notes1;  /* create empty database `notes1` */
\q  /* quit */
\c notes1;  /* connect to database named `notes1` */

/* after connected to a specific database inside sql-cli */
\dt /* list tables in database */
SELECT * FROM <table_name>   /* view table data */
SELECT * FROM "UserCred"
/* remeber to wrap table name with string-quotes, without it UserCred will be parsed as usercred */

/* update value of column `totalPrice` to 15, at row with `id` is equal to 4, in `Cart` table */
SELECT "Cart"
SET "totalPrice" = 15
WHERE "id" = 4;
```
