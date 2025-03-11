![image](https://github.com/user-attachments/assets/3755b674-a048-46f4-8203-11b9c9664f4b)



Queries Based on the Given Questions
   
a) Employees earning more than the Finance department's average salary

SELECT First_Name, Last_Name, Dept_Name 
FROM Employee e
JOIN Department d ON e.Dept_No = d.Dept_No
WHERE Salary > (
    SELECT AVG(Salary) 
    FROM Employee 
    WHERE Dept_No = (SELECT Dept_No FROM Department WHERE Dept_Name = 'Finance')


b) Employees working on more than 2 projects controlled by the R&D department

SELECT e.First_Name, e.Last_Name, d.Dept_Name
FROM Employee e
JOIN Works_On w ON e.SSN = w.Employee_SSN
JOIN Project p ON w.Proj_No = p.Proj_No
JOIN Department d ON p.Controlled_By = d.Dept_No
WHERE d.Dept_Name = 'R&D'
GROUP BY e.First_Name, e.Last_Name, d.Dept_Name
HAVING COUNT(w.Proj_No) > 2;


c) List all ongoing projects controlled by departments

SELECT Proj_Name, Controlled_By 
FROM Project;


d) Supervisors managing more than 3 employees who completed at least 1 project

SELECT Supervisor_SSN, COUNT(DISTINCT SSN) AS Supervised_Count
FROM Employee
WHERE SSN IN (SELECT Employee_SSN FROM Works_On)
GROUP BY Supervisor_SSN
HAVING COUNT(DISTINCT SSN) > 3;


e) Dependents of employees who worked exactly 101 hours

SELECT d.Dependent_Name 
FROM Dependent d
WHERE d.Employee_SSN IN (
    SELECT Employee_SSN 
    FROM Works_On 
    GROUP BY Employee_SSN
    HAVING SUM(Hours) = 101
);


f) Employees and departments with projects in multiple cities

SELECT DISTINCT d.Dept_Name, e.First_Name, e.Last_Name, p.Proj_Name
FROM Employee e
JOIN Works_On w ON e.SSN = w.Employee_SSN
JOIN Project p ON w.Proj_No = p.Proj_No
JOIN Department d ON p.Controlled_By = d.Dept_No
WHERE p.Location LIKE '%,%';
