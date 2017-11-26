sqlite3 test.db=> creates or use a database
.schema =>all tables schema

CREATE TABLE student(sid int PRIMARY KEY not null, name varchar(30) not null, age int);
CREATE TABLE prof(pid int PRIMARY KEY not null, name varchar(30) not null, room int, tel long);
CREATE TABLE attend(sid int,lid int, foreign key(sid) references student(sid), foreign key(lid) references lecture(lid), primary key(sid, lid));
CREATE TABLE lecture(lid int PRIMARY KEY not null, name varchar(30) not null, room int, pid int, foreign key(pid) references prof(pid));

create table attend(matrnr int references student(matrnr) not null, lnr int references lecture(lnr) not null, sem char(4),constraint attend_pk primary key (matrnr,lnr));


Table column details
PRAGMA table_info('lecture');

Enable displaying of column names in the table header
.header ON

Rename a table
alter table lecture rename to lecture_old;

Copy from a table
insert into  attend(matrnr, lnr,sem) select matnr,lnr,sem from attend_old;


See the sqlite env variable, use PRAGMA command.
e.g. foregin key constraint enable/disable
pragma foreign_keys;
[console o/p] foreign_keys 0
To set the foreign key constaint
pragma foreign_keys=1;

delete a table 
drop table lecture_old;

For every new connection to the database:
.header ON
pragma foreign_keys =1;

Relational Operator AND

select student.sid SID, student.name as Student, attend.lid as CourseId, lecture.name as Subjects,prof.name as Prof from student, attend,lecture, prof where attend.sid=student.sid and attend.lid=lecture.lid and lecture.pid=prof.pid;

SID|Student|CourseId|Subjects|Prof
1|kalinga|1701|Algorithms|Dietzfelbinger
1|kalinga|1702|Information System|K.Satller
1|kalinga|1703|System Engineering|Zimmerman
2|Sreeni|1701|Algorithms|Dietzfelbinger
2|Sreeni|1702|Information System|K.Satller
3|Subhesh|1701|Algorithms|Dietzfelbinger
3|Subhesh|1702|Information System|K.Satller
4|Antara|1701|Algorithms|Dietzfelbinger
4|Antara|1702|Information System|K.Satller
5|Sonali|1701|Algorithms|Dietzfelbinger
6|Kashvi|1701|Algorithms|Dietzfelbinger
6|Kashvi|1702|Information System|K.Satller
6|Kashvi|1703|System Engineering|Zimmerman
7|Tejas|1702|Information System|K.Satller
7|Tejas|1703|System Engineering|Zimmerman
8|Rahul|1702|Information System|K.Satller
8|Rahul|1703|System Engineering|Zimmerman
9|Daxesh|1703|System Engineering|Zimmerman

# PROBLEM 1:
alter table lecture rename to lecture_old;
update "lecture_old" set pnr=1 where lnr=1;
Error: no such table: main.pnr

# Problem 2:
int takes alphanumeric without any shout?!
CREATE TABLE lecture(lnr int primary key not null, name varchar(30) not null, room int, pnr int references professor(pnr) );

insert into  lecture (lnr, name,room, pnr) values(1,"Information System","H1510",1),(2, "Internet Security", "K1234",2),(3, "Data Management", "Hu1111",1);

Formulate the following queris in SQL:

• List the names of all students that attend a lecture in WS17.
sqlite> select name as NAME from student, attend where sem="ws17" and
		student.matrnr=attend.matrnr;
NAME
Amy
Bob
Charly
Ann

• List the names of all students that ever attended Information Systems.
sqlite> select s.name as NAME from student as s, attend ,lecture as l where l.name="Information 		System" and l.lnr=attend.lnr and attend.matrnr=s.matrnr;
NAME
Amy
Ann
Bob
Charly

• Count the number of students in Internet Security in SS17.
sqlite> select count(\*) COUNT from attend as a, lecture as l where l.name="Internet Security" 			and l.lnr=a.lnr and a.sem="ss17"; 
COUNT
2

• Which professors do not give any lecture?
sqlite> select name from professor where professor.pnr not in (select pnr from lecture);
Saake
Heuer

• Count the number of lectures per professor.
sqlite> select p.name as PROF, count (l.pnr) as 'No.Lecture' from lecture as l, professor as p  		where p.pnr=l.pnr group by l.pnr;
PROF|No.Lecture
Sattler|2
Schaefer|1

'where' triggers a Natural join so leaving those not common in both the table
so there are missing professors with 0 as No.Lecture. For that we need to apply LEFT JOIN ON

sqlite> select p.name as PROF, count (l.pnr) as 'No.Lecture' from professor as p LEFT JOIN 				lecture as l  on  p.pnr=l.pnr group by p.pnr;
PROF|No.Lecture
Sattler|2
Schaefer|1
Saake|0
Heuer|0

Additional Problem 1. Which professors give lecture?
sqlite> select distinct p.name from professor as p, lecture as l where l.pnr = p.pnr;
name
Sattler
Schaefer
