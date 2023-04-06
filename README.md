# Student-Information-Database
This project is the final project of the CMPE341 Database Design and Management course. It is designed for the student information database of a private university in Ankara. In this project, there are ER Diagram and SQL codes of the database.

## Contents
1. Introduction
2. System Analysis
   - Case Description
3. Requirements
   - Student
   - Teacher
   - Payment
   -Course
   -Department
4. ER Diagram
5. ER Diagram Mapping
6. Normalization
7. Create Tables
8. Insert
9. Queries

### Introduction
We will design the database system of the private university to be opened in Ankara. Since it is a new university, there has been no previous study on this subject. This newly opened university hosts 500 academicians and 2000 students in its first academic years. That's why this university needs a database system that can easily direct teachers and students. In the student information data model, we aim to facilitate the daily life of the student. This system allows students to review and evaluate their academic performance and manage their responsibilities. At the same time, students' advisor teachers can access and communicate with students through this system. The database includes 5 tables and the names are as follows: Student, Teacher, Department, Payment, Course.

### System Analysis
#### Case Description
There were some database systems used in this university, but students could not get efficiency from these systems while using them. For example, students could not access payment information and had to search for student jobs. With the database we built, we provided students with easy access to payment information, course information, and grades.

### Requirements
#### Student
- 	Each student has a composite Name, composite AddressID, StudentId(primary key), DOB, Age(derived attribute) and Email and Class.
* 	Each student must register a department. 
+ 	Each student must has grade report in many course.Also relation which is HasGradeReport has 3attributes like LetterGrade, NumericGrade and GPA(derived attribute).
- 	Each student might pay many payment 

#### Teacher
- 	Each teacher has a Name, composite AddressID, SSN(primary key) and TeMail.
* 	Teacher might give many course.
+ 	Teacher must have one department.

#### Payment
- 	Each payment has Class,  TotalAmount, PaymentDate, CardDetails(primary key) and Semester.
* 	Payment might pay by many students.

#### Course
- 	Each course has CourseName, CourseID(primary key), Credit and Department.
* 	Each course must give by exactly one teacher.
+ 	Course must include exactly one department.
- 	Course must has grade report for many student.

#### Department
- 	Each department has DepName(primary key) and Schedule.
* 	Each deparment might has many teacher.
+ 	Department might register by many student.
- 	Department must include many course.

### ER Diagram
![image](https://user-images.githubusercontent.com/87394685/230311808-dd8f0ecd-b976-4d5d-8cdf-4d72a8f91a8f.png)

### ER Diagram Mapping
#### Step 1
Teacher (SSN,  PostalCode, City, Country, Street, Name,TeMail,DepName,AddressID) (DepName FK refers to Department) (AddressID FK refers to Address)
Student (StudentId,  DOB, Age, PostalCode, City, Country, Street,FName, Minit, LName, Email, Class,AddressID,DepName) (AddressID FK refers to Address) (DepName FK refers to Department)
Department (DepName, Schedule)
Course (CourseID, CourseName, Credit, Department,SSN,DepName)  (SSN FK refers to Teacher) (DepName FK refers to Department) 
Payment (CardDetails, TotalAmount, PaymentDate,Class Semester)

#### Step 2
Pay(StudentId,CardDetails, DOB, Age, PostalCode, City, Country, Street,FName, Minit, LName, E-mail, Class, TotalAmount, PaymentDate,Class, Semester) (StudentId FK refers to Student) ( CardDetails FK refers to Payment)
Has_Grade_Report(StudentId, CourseID, DOB, Age, PostalCode, City, Country, Street,FName, Minit, LName, E-mail, Class, CourseName, Credit, Department, LetterGrade,GPA,NumericGrade) (StudentId FK refers to Student) (CourseID FK refers to Course) 

### Normalization
| Table | FDs |
| --- | --- |
| Students | StudentId -> DOB, Age, Class,Email  AddressID->PostalCode,City,Country,Street  Name-> FName, Minit, LName|
| Student Payments |  StudentId -> Payment |
| Course | CourseID-> CourseName, Credit, Department |
| Department | DepName->Schedule |
| Teacher | SSN-> Name,TeMail  AddressID->PostalCode,City,Country,Street |
| Payment | CardDetail->TotalAmount, PaymentDate,Class,Semester |

### Create Tables
CREATE TABLE ADDRESS(

Id int NOT NULL,

PostalCode int,

City VARCHAR (30),

Country VARCHAR(30),

Street VARCHAR(50),

PRIMARY KEY(Id));

CREATE TABLE DEPARTMENT(

DepName VARCHAR(50) NOT NULL,

Schedule VARCHAR (500) NOT NULL,

PRIMARY KEY(DepName)
);

CREATE TABLE TEACHER(

SSN NUMBER NOT NULL,

Name VARCHAR(20) NOT NULL,

AddressID int NOT NULL,

TeMail VARCHAR(20) NOT NULL,

DepName VARCHAR(50) NOT NULL,

PRIMARY KEY (SSN),

FOREIGN KEY(AddressID) REFERENCES ADDRESS(Id),

FOREIGN KEY(DepName) REFERENCES DEPARTMENT(DepName)
);

CREATE TABLE STUDENT(

StudentId int NOT NULL,

Fname VARCHAR(20) NOT NULL,

Minit VARCHAR(20),

LName VARCHAR(20) NOT NULL,

Email VARCHAR (30) NOT NULL,

Class int NOT NULL,

Age int,

DOB int,

DepName VARCHAR(50) NOT NULL,

AddressID int NOT NULL,

PRIMARY KEY (StudentId),

FOREIGN KEY(AddressID) REFERENCES Address(Id),

FOREIGN KEY(DepName) REFERENCES DEPARTMENT(DepName)
);

CREATE TABLE COURSE(

CourseID int NOT NULL,

CourseName VARCHAR (50) NOT NULL,

Credit int NOT NULL,

DepName VARCHAR(50) NOT NULL,

SSN NUMBER NOT NULL,

PRIMARY KEY (CourseID),

FOREIGN KEY(DepName) REFERENCES DEPARTMENT(DepName),

FOREIGN KEY(SSN) REFERENCES TEACHER(SSN)
);

CREATE TABLE HAS_GRADE_REPORT(

ReportID int NOT NULL,

CourseID int NOT NULL,

StudentId int NOT NULL,

LetterGrade VARCHAR(10) NOT NULL,

GPA int,

NumericGrade int NOT NULL,

PRIMARY KEY(ReportID),

FOREIGN KEY(CourseID) REFERENCES COURSE(CourseID),

FOREIGN KEY(StudentId) REFERENCES STUDENT(StudentId)
);

CREATE TABLE PAYMENT(

CardDetails NUMBER NOT NULL,

Semester VARCHAR(50) NOT NULL,

Class int NOT NULL,

PaymentDate int NOT NULL,

TotalAmount int NOT NULL,

PRIMARY KEY (CardDetails)
);

CREATE TABLE PAY(

CardDetails NUMBER NOT NULL,

StudentId int NOT NULL,

PRIMARY KEY(CardDetails,StudentId),

FOREIGN KEY(CardDetails) REFERENCES PAYMENT(CardDetails),

FOREIGN KEY(StudentId) REFERENCES STUDENT(StudentId)
);

### Insert
INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(1,60397,'ANKARA','TURKIYE','ELCI');

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(2,45896,'IZMIR','TURKIYE','NENEHATUN');

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(3,12547,'SIVAS','TURKIYE','FILISTIN') ;

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(4,36452,'CORUM','TURKIYE','KARANFIL');

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(5,28417,'YOZGAT','TURKIYE','LISE');  

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(6,66998,'ANTALYA','TURKIYE','CIKIKCILAR');

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(7,17254,'ANKARA','TURKIYE','SULUHAN');

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(8,52814,'KIRKLARELI','TURKIYE','MITHATPASA');

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(9,13952,'MARDIN','TURKIYE','DIKMEN');

INSERT INTO address(Id,PostalCode,City,Country,Street) 

VALUES(10,11478,'KAHRAMANMARAS','TURKIYE','AYDOGDU');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('SE','Database Management,Internet Programming,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('ISE','Database Management,Internet Programming,Computer Science'); 

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('ME','Resistance,Internet Programming,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('CMPE','Database Management,Internet Programming,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('CE','Database Management,Computer Programming,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('IE','Probability and Statistics,Internet Programming,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('AE','Database Management,Linear Algebra,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('EE','Database Management,Internet Programming,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('MATE','Database Management,Internet Programming,Math');

INSERT INTO DEPARTMENT(DepName,Schedule)

VALUES('ENE','Database Management,Multimedia Systems,Math');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(11111111111,'Sacip',1,'sacip@gmail','SE');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(22222222222,'Serpil',2,'serpil@hotmail','ISE');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(33333333333,'Ahmet',3,'ahmet@hotmail','ME');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(44444444444,'Tuğkan',4,'tuğkan@hotmail','CMPE');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(55555555555,'Işıl',5,'isil@hotmail','CE');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(66666666666,'Necla',6,'necla@hotmail','IE');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(77777777777,'Emel',7,'emel@gmail','AE');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName)

VALUES(88888888888,'Münevver',8,'munevver@gmail','EE');

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(99999999999,'Lara',9,'lara@gmail','MATE'); 

INSERT INTO teacher (SSN,Name,AddressID,TeMail,DepName) 

VALUES(12345678901,'Kerim',10,'kerim@gmail','ENE');


INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610001,'Pınar','Aslı','Türk','pinar@gmail',3,25,1995,'SE',1);            

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610002,'Eylül','','Özay','eylül@gmail',2,24,1996,'ISE',2);

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610003,'Elif','Burcu','Acar','eburcu@gmail',3,23,1997,'ME',3);

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610004,'Mert','Ali','Özdemir','mertali@gmail',1,25,1995,'CMPE',4);

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610005,'Adalet','Funda','Oğuz','funda@gmail',4,26,1994,'CE',5);

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610006,'Kadir','','Ata','kadiru@gmail',3,21,1999,'IE',6);


INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610007,'Akın','','Sarı','akıns@gmail',3,26,1994,'AE',7);

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610008,'Zeynep','','Yılmaz','zeynepy@gmail',1,23,1997,'EE',8);

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID)

VALUES(17243610009,'Canan','','Öztürk','canan@gmail',2,25,1995,'MATE',9);

INSERT INTO student (StudentId,Fname,Minit,Lname,Email,Class,Age,DOB,DepName,AddressID) 

VALUES(17243610000,'Orhan','','Karademir','orhan@gmail',1,25,1995,'ENE',10);


INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(11,'Internet Programming',5,'SE',11111111111);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(12,'Database Design',6,'ISE',22222222222);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(13,'Computer Science',7,'ME',33333333333);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN)

VALUES(14,'Computer Programming',4,'CMPE',44444444444);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(15,'Math',8,'CE',55555555555);


INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(16,'Linear Algebra',3,'IE',66666666666);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(17,'Resistance',7,'AE',77777777777);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(18,'Fluid mechanics',8,'EE',88888888888);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(19,'Probability and Statistics',6,'MATE',99999999999);

INSERT INTO course(CourseID,CourseName,Credit,DepName,SSN) 

VALUES(20,'Multimedia Systems',4,'ENE',12345678901);


INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(31,11,17243610001,'AA',4,100);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(32,12,17243610002,'BA',3,85);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(33,13,17243610003,'CC',3,70);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(34,14,17243610004,'CB',3,75);


INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(35,15,17243610005,'DC',2,55);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade)

VALUES(36,16,17243610006,'CC',3,72);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(37,17,17243610007,'DD',2,50);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade)

VALUES(38,18,17243610008,'CB',2,65);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(39,19,17243610009,'BB',3,80);

INSERT INTO has_grade_report(ReportID,CourseID,StudentId,LetterGrade,GPA,NumericGrade) 

VALUES(40,20,17243610000,'AA',4,90); 


INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667781,'Spring',3,2020,10000);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667782,'Fall',2,2021,5000);


INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667783,'Fall',3,2019,10000);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount)

VALUES(1122334455667784,'Spring',1,2020,10000);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667785,'Spring',4,2021,10000);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667786,'Fall',3,2020,10000);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667787,'Spring',3,2018,7500);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667788,'Fall',1,2019,5000);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667789,'Spring',2,2017,10000);

INSERT INTO payment(CardDetails,Semester,Class,PaymentDate,TotalAmount) 

VALUES(1122334455667780,'Fall',1,2018,7500);


INSERT INTO pay(CardDetails,StudentId)

VALUES(1122334455667781,17243610001);

INSERT INTO pay(CardDetails,StudentId)

VALUES(1122334455667782,17243610002);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667783,17243610003);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667784,17243610004);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667785,17243610005);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667786,17243610006);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667787,17243610007);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667788,17243610008);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667789,17243610009);

INSERT INTO pay(CardDetails,StudentId) 

VALUES(1122334455667780,17243610000);

### Queries
SELECT STUDENT.Fname,Student.StudentId, DEPARTMENT.DepName

FROM STUDENT

LEFT JOIN DEPARTMENT ON DEPARTMENT.DepName=STUDENT.DepName

ORDER BY STUDENT.Fname;


SELECT TEACHER.Name,TEACHER.Temail,COURSE.CourseID,COURSE.CourseName,COURSE.Credit,COURSE.DepName

FROM TEACHER

LEFT JOIN COURSE ON TEACHER.SSN=COURSE.SSN

ORDER BY TEACHER.Name;


SELECT STUDENT.StudentId,Fname,Lname,ADDRESS.City,ADDRESS.PostalCode,ADDRESS.Country

FROM STUDENT

INNER JOIN ADDRESS ON student.addressıd=ADDRESS.Id

ORDER BY STUDENT.Fname;

SELECT COURSE.CourseName,HAS_GRADE_REPORT.StudentId,HAS_GRADE_REPORT.GPA

FROM COURSE

FULL JOIN HAS_GRADE_REPORT

ON COURSE.CourseID=HAS_GRADE_REPORT.CourseID;


SELECT STUDENT.StudentId,HAS_GRADE_REPORT.GPA,STUDENT.Class

FROM STUDENT

INNER JOIN HAS_GRADE_REPORT

ON STUDENT.StudentId=HAS_GRADE_REPORT.StudentId;


SELECT HAS_GRADE_REPORT.LetterGrade,STUDENT.StudentId,COURSE.CourseID

FROM(( HAS_GRADE_REPORT

INNER JOIN STUDENT ON HAS_GRADE_REPORT.StudentId =STUDENT.StudentId)

INNER JOIN COURSE ON HAS_GRADE_REPORT.CourseID=COURSE.CourseID);

SELECT StudentId,LetterGrade,GPA,NumericGrade

FROM HAS_GRADE_REPORT

ORDER BY NumericGrade DESC;


SELECT CourseName,Credit,DepName,HAS_GRADE_REPORT.LetterGrade 

FROM COURSE,HAS_GRADE_REPORT

WHERE LetterGrade='AA';


SELECT StudentId

FROM HAS_GRADE_REPORT

WHERE GPA=(SELECT MAX(GPA)

FROM HAS_GRADE_REPORT);


SELECT StudentId, PaymentDate,TotalAmount

FROM PAY,PAYMENT

WHERE TotalAmount=10000

AND (PaymentDate=2019 OR PaymentDate=2020);








