![image](https://github.com/user-attachments/assets/3755b674-a048-46f4-8203-11b9c9664f4b)

SELECT e.first_name, e.last_name, d.dept_name
FROM Employee e
JOIN Department d ON e.dept_id = d.dept_id
WHERE e.salary > (
    SELECT AVG(salary)
    FROM Employee e2
    JOIN Department d2 ON e2.dept_id = d2.dept_id
    WHERE d2.dept_name = 'Finance'
);


SELECT e.first_name, e.last_name, d.dept_name
FROM Employee e
JOIN Works_On w ON e.emp_id = w.emp_id
JOIN Project p ON w.proj_id = p.proj_id
JOIN Department d ON p.dept_id = d.dept_id
WHERE d.dept_name = 'R&D'
GROUP BY e.emp_id
HAVING COUNT(w.proj_id) > 2;


SELECT p.proj_name, d.dept_name
FROM Project p
JOIN Department d ON p.dept_id = d.dept_id;


SELECT e.supervisor_id, s.first_name, s.last_name, COUNT(DISTINCT e.emp_id) AS supervised_employees
FROM Employee e
JOIN Employee s ON e.supervisor_id = s.emp_id
JOIN Works_On w ON e.emp_id = w.emp_id
GROUP BY e.supervisor_id
HAVING COUNT(DISTINCT e.emp_id) > 3;


SELECT d.dep_name, e.first_name, e.last_name
FROM Dependent d
JOIN Employee e ON d.emp_id = e.emp_id
JOIN Works_On w ON e.emp_id = w.emp_id
GROUP BY d.dep_id
HAVING SUM(w.hours_worked) = 101;

f) List the department and employee details whose project is in more than one city.

SELECT DISTINCT d.dept_name, e.first_name, e.last_name, p.proj_name
FROM Employee e
JOIN Works_On w ON e.emp_id = w.emp_id
JOIN Project p ON w.proj_id = p.proj_id
JOIN Department d ON e.dept_id = d.dept_id
WHERE p.location LIKE '%,%';

