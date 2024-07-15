```sql
create table info1(
  id int auto_increment primary key,
  name varchar(30) not null,
  amount decimal(14,4) not null,
  completed bool not null default false,
  status enum('verified', 'rejected', 'pending') not null default 'pending',
  wallet_address varchar(42) null default null,
  created_at timestamp not null default current_timestamp,
  expiry_at timestamp not null
);
rename table info1 to info3;
drop table info1;

alter table info 
modify column name varchar(42) not null;  
alter table info1 
add column recipient varchar(42) not null default '' after amount; -- `after amount` == put column after column 'amount'

insert into info1 (name, amount, expiry_at)
values ('task m1', 130.33, date_add(current_timestamp, interval 1 day)); -- expires after 1 day from current time

alter table info1
add unique index unique_info1_name(name);


create table info2(
   info1_id int not null
);

alter table info2 
add constraint fk_info2_info1
foreign key (info1_id) references info1(id);  
-- forign key have table scope (foreign needs to be within table, but need not to be unique in whole database)


------------------------------ JOIN ON ----------------------------------------------
SELECT columns  -- select given columns
FROM table1     -- from table1
JOIN table2     -- wait, first look at table2
ON table1.column = table2.column; -- consider rows only where table1's column value is equal to table2's column value 
-- it's a common practice to mention table mentioned after "FROM" to use on left side of "=" after "ON"

------------------------------ multiple foreign keys -------------------------------------------------------
-- suppose each row in info2 references to multiple rows in info1
CREATE TABLE joint_table (
    info2_id INT,
    info1_id INT,
    FOREIGN KEY (info2_id) REFERENCES info2(id),
    FOREIGN KEY (info1_id) REFERENCES info1(id),
    PRIMARY KEY (info2_id, info1_id)
);

-- now to get all info1 rows referenced in a single info2 row, we can use following query
SELECT 
    info1.id,
    info1.name
FROM 
    info1
JOIN 
    joint_table ON info1.id = joint_table.info1_id 
WHERE 
    joint_table.info2_id = [specific_info2_id];
```
