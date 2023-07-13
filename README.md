# University Database using Microsoft Access
A relational database made in Access which is used for structuring randomly generated student data. 
The process for making this database is described as follows:
- Created relationships and constraints using an Entity Relationship Diagram.
- Created a relational schema to meet the business needs of the theoretical university.
- Developed the entity relationship diagram and relational schema in LaTeX using the TiKZ `er` package. 
- Implemented the entity relationship diagram and relational schema in Microsoft Access.
- Manually added randomly generated data into the database.
- Developed SQL queries to fetch data concerning student's major and minor (LEFT JOIN), all the pre-requisites offered by the university (LEFT JOIN), courses including sections professors are teaching and a specific courses students majoring in CS are taking using AND and WHERE clause, use aggregate function COUNT, GROUP BY, and ORDER BY to create table count of all the students and their majors at the college.

The entity relationship diagram and the relational schema are developed in LaTeX.
I cannot share the code for the ERD because I by mistake over-wrote the original code.

The entity relationship diagram for the project as shown in Fig. 1.
<p align="center">
  <img src="https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/e4b125a5-4254-42ab-96a5-3283ee5208c0" >
</p>
<p align="center""><strong>Figure 1:</strong><i> Illustration of ERD for a University database coded in the LaTeX document scripting language.</i></p>

The relational schema for the project as shown in Fig. 2.

<p align="center">
  <img src="https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/c4ded4e8-302a-471f-94a1-e6157fce3ab3" >
</p>
<p align="center""><strong>Figure 2:</strong><i> Illustration of Relational Schema for a University database coded in the LaTeX document scripting language.</i></p>

The fact and dimension table implemented in Microsoft Access, Fig. 3.

<p align="center">
  <img src="https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/ce15d603-7e15-49d1-83c4-dbbbe68807da" >
</p>
<p align="center""><strong>Figure 3:</strong><i> Relationships tab view in Access for the database.</i></p>

Here are 5 SQL queries I ran on the data.

<strong>Query #1:</strong> We want the name of the student, their major and their minor.

```sql
SELECT student.ssn, student.fname AS [first name], student.lname AS [last name], minor.code AS minor, major.code AS major
FROM (Student LEFT JOIN Minor ON student.ssn = minor.ssn) LEFT JOIN major ON student.ssn = major.ssn;
```
![query1_output](https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/0c73d7f5-88fc-4b55-acae-871b534f0b39)

<strong>Query #2:</strong> Which professor is teaching what course and section e.g., `CS5000-080` in the Fall semester.
```sql
SELECT ssn, iname AS name, course.number & '-' & section.number AS [course-section]
FROM Instructor, Course, [Section], Teaches
WHERE ssn = issn 
AND course.number = section.cnumber 
AND teaches.number = section.number
AND section.semester = 'Fall';
```
![query2_output](https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/53612aab-6a0a-4c1f-aa68-79d49b51c667)

<strong>Query #3:</strong> Table of computer science majoring students that are enrolled in `CS4400` including their degree and class rank.
```sql
SELECT student.ssn AS ssn, fname AS [first name], lname AS [last name], program AS [degree program], class, code AS major
FROM Student, Enrolls, Major, Course, [Section]
WHERE student.ssn = major.ssn
AND student.ssn = enrolls.ssn
AND enrolls.snumber = section.number
AND course.number = cnumber
AND cnumber = '4400'
AND dcode = 'CS';
```
![query3_output](https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/6158658a-fce1-4fe8-abb0-8827afdbda58)

<strong>Query #4:</strong> List of pre-requisites for all courses offered in the university.
```sql
SELECT Course.number AS [course number], Course.coursename AS [course name], Course_Prerequisite.prerequisite AS [pre-requisite], Course.offering_dept AS [department name]
FROM Course LEFT JOIN Course_Prerequisite ON Course.number = Course_Prerequisite.Number;
```
![query4_output](https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/f11e8453-91b5-4915-b00d-ee32782cd68c)

<strong>Query #5:</strong> A count of the students that are majoring in each major offered by the college.
```sql
SELECT count(student.ssn) AS students, major.code AS major
FROM Student, Major
WHERE student.ssn = major.ssn
GROUP BY major.code
ORDER BY count(student.ssn);
```
![query5_output](https://github.com/miahj1/University-Database-using-Microsoft-Access/assets/84815985/cb490a49-d717-4c3e-a5fe-293b550e7470)
