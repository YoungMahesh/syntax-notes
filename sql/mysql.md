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
foreign key (info1_id) references info1(id);  -- forign key have table scope (foreign needs to be within table, but need not to be unique in whole database)
```
