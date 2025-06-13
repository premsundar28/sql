creating a table
create table person (id int primary key auto_increment , name varchar(25) , address varchar(100));

inserting into the table
insert into person (id , name , address) values(0,"raghu" , "hello world");
as their is an auto increment we can skip the id part of the query
insert into person ( name , address) values("raghu2" , "hello world");


selecting table data
select * from person

description about the table
desc table_name
desc person

alter table
alter table person add gender varchar(10);

drop table 
drop table person

views
create view v2 as select * from person;

dropping the views
drop view v1;

renaming the table
rename table person to p1;

stored procedure

create procedure getP1() 
begin 
select * from p1;
end ;

calling stored procedure 
call p1;

stored procedure with params:

create procedure getP3(in in_id int , out out_name varchar(90)) 
begin 
select name into out_name from p1 where id = in_id;
end ;

call getp3(1,@name);
select @name;

triggers:
CREATE TRIGGER loggg3
BEFORE INSERT ON p1
FOR EACH ROW
BEGIN
    INSERT INTO log2 VALUES ('logged');
END;


dropping trigger:
drop trigger loggg3

quering all the triggers:
show triggers;





