
# SQL Queries

## Problem 1: EMP Table Queries

### Table Structure:
**EMP(empno, deptno, enam, salary, Designation, joingdat, DOB, city)**

### Queries:

1. **Display employees' name and number in an increasing order of salary**
```sql
SELECT enam, empno
FROM EMP
ORDER BY salary ASC;
```

2. **Display employee name and employee number department-wise**
```sql
SELECT deptno, enam, empno
FROM EMP
ORDER BY deptno;
```

3. **Display the total salary of all employees**
```sql
SELECT SUM(salary) AS Total_Salary
FROM EMP;
```

4. **Display the number of employees department-wise**
```sql
SELECT deptno, COUNT(*) AS Number_of_Employees
FROM EMP
GROUP BY deptno;
```

5. **Display employee names having experience more than 3 years**
```sql
SELECT enam
FROM EMP
WHERE DATEDIFF(CURDATE(), joingdat) / 365 > 3;
```

6. **Display employee names starting with 'S' and working in deptno 1002**
```sql
SELECT enam
FROM EMP
WHERE enam LIKE 'S%' AND deptno = 1002;
```

---

## Problem 2: STUD Table Queries

### Table Structure:
**STUD(rollno, name, sub1, sub2, sub3)**

### Queries:

1. **Display the name of the student who got minimum marks in sub1**
```sql
SELECT name
FROM STUD
WHERE sub1 = (SELECT MIN(sub1) FROM STUD);
```

2. **Display the name of the student who obtained the highest marks in sub3**
```sql
SELECT name
FROM STUD
WHERE sub3 = (SELECT MAX(sub3) FROM STUD);
```

3. **Display the number of students who failed in sub2**
*Assume passing marks are 40*
```sql
SELECT COUNT(*) AS Failed_Students
FROM STUD
WHERE sub2 < 40;
```

4. **Find the total marks of sub1 for all students**
```sql
SELECT SUM(sub1) AS Total_Sub1_Marks
FROM STUD;
```

---

## Problem 3: Employee and Company Schema Queries

### Schema:
- **employee(employee-name, street, city)**
- **works(employee-name, company-name, salary)**
- **company(company-name, city)**
- **manages(employee-name, manager-name)**

### Queries:

1. **Find the names, street addresses, and cities of residence for all employees who work for 'First Bank Corporation' and earn more than $10,000.**
```sql
SELECT e.employee_name, e.street, e.city
FROM employee e
JOIN works w ON e.employee_name = w.employee_name
WHERE w.company_name = 'First Bank Corporation' AND w.salary > 10000;
```

2. **Find the names of all employees in the database who live in the same cities as the companies for which they work.**
```sql
SELECT DISTINCT e.employee_name
FROM employee e
JOIN works w ON e.employee_name = w.employee_name
JOIN company c ON w.company_name = c.company_name
WHERE e.city = c.city;
```

3. **Find the names of all employees in the database who live in the same cities and on the same streets as do their managers.**
```sql
SELECT e.employee_name
FROM employee e
JOIN manages m ON e.employee_name = m.employee_name
JOIN employee mgr ON m.manager_name = mgr.employee_name
WHERE e.city = mgr.city AND e.street = mgr.street;
```

4. **Find the names of all employees in the database who do not work for 'First Bank Corporation'.**
```sql
SELECT employee_name
FROM works
WHERE company_name != 'First Bank Corporation';
```

5. **Find the names of all employees in the database who earn more than every employee of 'Small Bank Corporation'.**
```sql
SELECT employee_name
FROM works
WHERE salary > ALL (
    SELECT salary
    FROM works
    WHERE company_name = 'Small Bank Corporation'
);
```

6. **Find all companies located in every city in which 'Small Bank Corporation' is located.**
```sql
SELECT DISTINCT c1.company_name
FROM company c1
WHERE NOT EXISTS (
    SELECT city
    FROM company c2
    WHERE c2.company_name = 'Small Bank Corporation'
    AND c2.city NOT IN (
        SELECT city
        FROM company c3
        WHERE c3.company_name = c1.company_name
    )
);
```

7. **Find the names of all employees who earn more than the average salary of all employees of their company.**
```sql
SELECT w.employee_name
FROM works w
JOIN (
    SELECT company_name, AVG(salary) AS avg_salary
    FROM works
    GROUP BY company_name
) avg_salaries ON w.company_name = avg_salaries.company_name
WHERE w.salary > avg_salaries.avg_salary;
```

---
