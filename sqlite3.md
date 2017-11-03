sqlite3 test.db=> creates or use a database
.schema =>all tables schema

CREATE TABLE student(sid int PRIMARY KEY not null, name varchar(30) not null, age int);
CREATE TABLE prof(pid int PRIMARY KEY not null, name varchar(30) not null, room int, tel long);
CREATE TABLE attend(sid int,lid int, foreign key(sid) references student(sid), foreign key(lid) references lecture(lid), primary key(sid, lid));
CREATE TABLE lecture(lid int PRIMARY KEY not null, name varchar(30) not null, room int, pid int, foreign key(pid) references prof(pid));


Table column details
PRAGMA table_info('lecture');

Enable displaying of column names in the table header
.header ON

Rename a table
alter table lecture rename to lecture_old;

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



