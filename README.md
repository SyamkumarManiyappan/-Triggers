# -Triggers
SQL Commands

## 10-Triggers
1. Create a table named teachers with fields id, name, subject, experience and salary and insert 8 rows.
2. Create a before insert trigger named before_insert_teacher that will raise an error “salary cannot be negative” if the salary inserted to the table is less than zero.
 3. Create an after insert trigger named after_insert_teacher that inserts a row with teacher_id,action, timestamp to a table called teacher_log when a new entry gets inserted to the teacher table. tecaher_id -> column of teacher table, action -> the trigger action, timestamp -> time at which the new row has got inserted.
4. Create a before delete trigger that will raise an error when you try to delete a row that has experience greater than 10 years.
5. Create an after delete trigger that will insert a row to teacher_log table when that row is decreate leted from teacher table.

   Answers :

   
create database School;
use school;
-- 1. Create a table named teachers with fields id, name, subject, experience and salary and insert 8 rows.

create table Teachers ( id int primary key, Name varchar(15) Not null , Subject varchar(15),experience int, Salary Int);
insert into Teachers (id,name,subject,experience,salary)
values (1,'Sowmya','Maths',12,80000),
		(2,'Susan','Malayalam',10,60000),
        (3,'Subash','Physics',7,58000),
        (4,'Sunil','Chemistry',2,45000),
        (5,'Midhun','IT',2,45000),
        (6,'Alice','English',3,48000),
        (7,'Akshay','Biology',3,49000),
        (8,'Helen','History',10,70000);
        
-- 2. Create a before insert trigger named before_insert_teacher that will raise an error “salary cannot be negative” if the salary inserted to the table is less than zero.
delimiter //
create trigger befor_insert_teacher
before insert on Teachers
for each row
begin
if new.salary < 0 then 
signal SQLstate '45000'
set message_text = 'salary cannot be negative';
end if;
end; //
delimiter ;

insert into teachers ( id,name,subject,experience,salary)
values(9,'Sharon','Hindi',4,-45000);

 -- 3. Create an after insert trigger named after_insert_teacher that inserts a row with teacher_id,action, timestamp to a table called teacher_log when a new entry gets inserted to the teacher table. 
 -- tecaher_id -> column of teacher table, action -> the trigger action, timestamp -> time at which the new row has got inserted.

create table teacher_log (
Teacher_id int,
action varchar (50),
timestamp datetime
);

delimiter //
create trigger after_insert_teacher
after insert on Teachers
for each row
begin
insert into teacher_log (Teacher_id,action,timestamp)
values ( new.id,'Insert',now());
end; //
delimiter ;

alter table teachers;
insert into teachers( id,name,subject,experience,salary)
values(10,'Karthika','Maths',6,55000);

select * from teacher_log;


-- 4. Create a before delete trigger that will raise an error when you try to delete a row that has experience greater than 10 years.

delimiter //
create trigger befor_delete_teacher
before delete on teachers
for each row
begin
if old.experience >10 then
signal sqlstate '45000'
set message_text ='cannot delete a teacher with more than 10 yeares experience';
end if;
end;
//
delimiter ;


delete from teachers where id=1;


-- 5. Create an after delete trigger that will insert a row to teacher_log table when that row is decreate leted from teacher table.

delimiter //
create trigger after_delete_teacher
after delete on teachers 
for each row
begin
insert into teacher_log(teacher_id,action,timestamp)
values (old.id,'Delete',now());
end; //
delimiter ;

delete from teachers where id=8;
select * from teacher_log;
