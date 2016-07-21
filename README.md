Harrison College Database

Courses k
- CourseId: Number (PK)
- CourseNo: Varchar (10)   ie: 'ENG 101' - could this be PK?
- Coursename: Varchar (20)
- Description: Varchar (50)
- SubjectCode: Varchar(5)  note: where is this used?
- DepartmentId: number (FK into Department)
- Credits: Number

Semesters s
- SemesterId: number;
- Semester: varchar(20)   ie: 2016 Summer, 2016 Spring, 2016 Fall
- CurrentSemester: number  ie: 1 indicates current semester, 0 not current

Classes c
- CRN: number (PK)
- CourseId: number (FK into Course)
- Semester: varchar(20)
- RoomNo: number (FK into Classroom.RoomNo)
- Instructorid: number  (FK into Instructor.EmpNo)

Classroom r
- RoomNo: number (PK)
- BuildingName: Varchar(20)
- MaxCapacity: number

Students s
- StudentId: number (PK)
- StudentName: Varchar(20)
- Major: Varchar(5)   note: use subjectcode, ie: CMSC, BUS, EE, ME, CE, PHY, MATH, ENG, NURS
- EntryYear: number

Instructors i
- Instructorid: number (PK)  (acts as the employee number)
- InstructorName: Varchar (20)
- DepartmentId: number (FK into Department.DepartmentId)
- OfficeNo: number

Enrollment e
- EnrollId: number (PK)
- StudentId: number (FK into Student.StudentNo)
- CRN: number (FK into Class.CRN)
- Grade: varchar(2)

Departments d
- DepartmentId: number (PK)
- DepartmentName: varchar(20)

-----------------------------------------------------------
SELECT s.StudentName, g.Grade 
	FROM Grades g
	WHERE s.StudentName = studentname and g.CRN = crn
	JOIN students s ON s.StudentId; = g.StudentId;
	
-----------------------------------------------------------
- Student creates an account
INSERT INTO Students (StudentName, Major, EntryYear)
	VALUES ('John Brown', 'CS', 2015);
-----------------------------------------------------------
- Students enroll in a class
INSERT INTO Enrollment (StudentID, CRN, Grade)
	VALUES (value1,value2,value3);
-----------------------------------------------------------
Students drop a class
DELETE FROM Enrollment
	WHERE StudentID = value and CRN=value;
-----------------------------------------------------------
- Students view their current schedule
-- get the currentsemester (store in session, logged in or not)
-- store the current student in the session upon logon
SELECT semester from Semesters
	WHERE CurrentSemester = 1;

SELECT e.CRN, k.coursename, k.description, c.subjectcode, i.instructorname, c.semester
	FROM Enrollment e
	INNER JOIN Classes c 
		ON e.CRN = c.CRN
	INNER JOIN Courses k
		ON c.courseid = k.courseid
	INNER JOIN Instructors i
		ON c.instructorid = i.instructorid
	WHERE e.StudentId = value and c.semester = currentsemester
-----------------------------------------------------------
- Students view transcript
SELECT e.CRN, k.coursename, k.description, k.subjectcode, i.instructorname, c.semester
	FROM Enrollment e
	INNER JOIN Classes c 
		ON e.CRN = c.CRN
	INNER JOIN Courses k
		ON c.courseid = k.courseid
	INNER JOIN Instructors i
		ON c.instructorid = i.instructorid
	WHERE e.StudentId = value
	ORDER BY c.semester DESC;
-----------------------------------------------------------
- View all courses
SELECT k.CourseName, k.Description, k.SubjectCode, d.DepartmentName
	FROM Courses k
	ORDER BY d.DepartmentName, k.SubjectCode, k.CourseName;
-----------------------------------------------------------
- view all classes in current semester 
-- get the current semester from the session

SELECT c.CRN, c.Semester, c.RoomNo, k.CourseName, k.Description, i.InstructorName
	FROM Classes c
	INNER JOIN Courses k
		ON k.CourseId = c.CourseId
	INNER JOIN Instructors ie
		ON i.InstructorId = c.InstructorId
	ORDER BY k.CourseName, c.CRN
	WHERE c.Semester = value;
-----------------------------------------------------------
- Instructors can view their classes in the current semester
-- get the current semester from the session, get instructor ID
SELECT c.CRN, c.Semester, c.RoomNo, k.CourseName, k.Description, i.InstructorName
	FROM Classes c
	INNER JOIN Courses k
		ON k.CourseId = c.CourseId
	INNER JOIN Instructors ie
		ON i.InstructorId = c.InstructorId
	ORDER BY k.CourseName, c.CRN
	WHERE c.Semester = value and i.InstructorName = value;

-----------------------------------------------------------
- get the roster of students in their classes from this semester 
- or previous semesters
-- get the instructor ID from session (saved at login by instructor)
-- prompt instructor to select the semester from drop-down menu 
--		(get all CRNs for that instructor for the selected semester)
--		prompt instructor to select the course name and CRN
-- 			display the class information below:

SELECT e.CRN, k.CourseName, s.StudentName, e.grade
	FROM Enrolllment e 
	INNER JOIN Classes c 
		ON c.CRN = e.CRN
	INNER JOIN Courses k 
		ON k.CourseId = c.CourseId
	INNER JOIN Students s 
		ON s.StudentId = e.StudentId
	WHERE c.InstructorId = value and c.CRN = value;	

-----------------------------------------------------------
-- view all course
SELECT * 
FROM Courses c
ORDER BY c.CourseName;
-----------------------------------------------------------
-- view all classes in current semester 
-- get the current semester from the session

SELECT c.CRN, c.Semester, c.RoomNo, k.CourseName, k.Description, i.InstructorName
	FROM Classes c
	INNER JOIN Courses k
		ON k.CourseId = c.CourseId
	INNER JOIN Instructors ie
		ON i.InstructorId = c.InstructorId
	ORDER BY k.CourseName, c.CRN
	WHERE c.Semester = value;



