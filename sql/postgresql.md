
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

