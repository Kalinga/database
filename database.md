* Create database `create schema <database_name>`
* Delete/Remove database `drop schema <database_name>`
* Create a table 'company_info' for the database 'mycompanies'

    
    CREATE TABLE `mycompanies`.`company_info` (
    `idCompany_name` VARCHAR(30) NOT NULL,
    `Company_Link` VARCHAR(45) NULL,
    `Company_location` VARCHAR(15) NULL,
    PRIMARY KEY (`idCompany_name`));

* Data entry, record insertion 

	INSERT INTO `mycompanies`.`company_info` (`idCompany_name`, `Company_Link`, `Company_location`) 
	VALUES ('iSeva Pvt Ltd.', 'http://www.e4e.com/', 'Bangalore');

* Change the length of a column

	ALTER TABLE `mycompanies`.`company_info` 
	CHANGE COLUMN `idCompany_name` `idCompany_name` VARCHAR(40) NOT NULL ;
	
* Version of MySQL
	SHOW VARIABLES LIKE 'version';
	
* MySql command line:
 	mysql -u root -p -h 127.0.0.1

* Basic commands:
 	create database employees;
 	use employees;
 	show tables;
 	create table ...
    describe <table_name>;

* Check the schema of the table    
mysql> describe employees;

    
    +------------+---------------+------+-----+---------+-------+
    | Field      | Type          | Null | Key | Default | Extra |
    +------------+---------------+------+-----+---------+-------+
    | emp_no     | int(11)       | NO   | PRI | NULL    |       |
    | birth_date | date          | NO   |     | NULL    |       |
    | first_name | varchar(14)   | NO   |     | NULL    |       |
    | last_name  | varchar(16)   | NO   |     | NULL    |       |
    | gender     | enum('M','F') | NO   |     | NULL    |       |
    | hire_date  | date          | NO   |     | NULL    |       |
    +------------+---------------+------+-----+---------+-------+    

* Conditional selection (and, or, not, is null, in, between x and y, not between x and y)


    select * from employees where last_name='penn' and first_name='Uzi'; 418 records
    select * from employees where last_name='penn' or first_name='Uzi'; 1 record
    select * from employees where birth_date between '1980-01-01' and '2000-01-01' limit 10;
    
* Default sorting is ASC
   
       select * from  company_info where <conditional> order by <col_name> DESC/ASC
       select * from employees where last_name='penn' order by first_name ASC;
       
* select with LIMIT
       
       select * from employees limit 50;
 
 * Update 
  
       mysql> update employees set first_name = 'Kalinga' where first_name='Gaurav' and last_name='Penn';
       delete from employees.employees where emp_no=486680;
       insert into employees values(303201080,'1988-09-04', 'kalinga', 'Ray','M', '2013-12-19');
       update employees set hire_date='2011-01-01' where first_name='Kalinga';
       delete from employees; -- Deletes all the records fr0m the table

* Functions: count, max, min, avg, sum

* Searching using LIKE

    
    select * from employees where first_name like 'ka%ga'; -- Returns Kalinga record
    
* Aliases

    select  first_name as VorName from employees limit 10;
    select emp_no as ID, first_name as NAME from employees where last_name LIKE 'ray' limit 10;
	
	+-----------+---------+
	| ID        | NAME    |
	+-----------+---------+
	| 303000080 | kalinga |
	+-----------+---------+    

	select  e.first_name as NAME, s.salary as SALARY
	from employees as e, salaries as s
	where e.emp_no=303201080 AND e.emp_no=s.emp_no limit 10	

	+---------+--------+
	| NAME    | SALARY |
	+---------+--------+
	| kalinga | 155555 |
	+---------+--------+

* Join Inner
 	
 	select e.first_name as NAME , s.salary as PAYMENT  
 	from employees as e 
 	inner join salaries as s 
 	on e.emp_no=s.emp_no 
 	AND e.emp_no=303201080 limit 100;

 	+---------+---------+
	| NAME    | PAYMENT |
	+---------+---------+
	| kalinga |  155555 |
	+---------+---------+

* Join Self

Can be very powerful sometimes:
Fetch all the names with similar last name as Name having first name as 'Georgi'
46471 rows in set (5.83 sec) For employees table

select e.emp_no, e.first_name as A_NAME, e1.emp_no,  e1.first_name as B_NAME, e1.last_name
from employees as e, employees as e1
where e.emp_no<>e1.emp_no AND e1.last_name=e.last_name AND e.first_name='Georgi' order by e1.last_name
 
* Need to check why below nested query does not work as expected

select e.emp_no, e.first_name, e.last_name, s.salary 
from employees e, salaries s 
where e.emp_no IN (select s.emp_no from salaries s where s.salary > 140000); 

* Union 

select e.emp_no, e.first_name, e.last_name 
from employees e 
where e.emp_no IN (select s.emp_no from salaries s 
				   where s.salary > 140000) 
union  
select e.emp_no, e.first_name, e.last_name 
from employees e 
where e.emp_no IN (select s.emp_no from salaries s 
where s.salary < 39000);

* Conditional Count


select count(first_name) from employees where last_name = 'ray';

* GROUP BY: The GROUP BY statement is often used with aggregate functions (COUNT, MAX, MIN, SUM, AVG)
  to group the result-set by one or more columns.

SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s); 

* Having: The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions.


select count(first_name), last_name 
from employees group by last_name 
having count(first_name) > 200 
order by count(first_name) asc;


* Exists:


select first_name from employees where exists(select from_date from salaries where emp_no=employees.emp_no and salary>150000);
16 rows in set (1.63 sec)

How to find the salaries for these 16 records?

phpcademy.org 
thenewboston.org