<H1>Chapter 5 practice problem solutions of Oracle PL-SQL book</H1>

-- 5.1 a</br>
-- For each employee print last name, salary, and job title.
```sql
SELECT last_name, salary, job_title
FROM employees
         LEFT JOIN jobs ON employees.job_id = jobs.job_id;
```

-- 5.1 b</br>
-- For each department, print department name and country name it is situated in.
```sql
SELECT department_name, country_name
FROM departments
         LEFT JOIN locations l ON departments.location_id = l.location_id
         LEFT JOIN countries c2 ON c2.country_id = l.country_id;
```

-- 5.1 c</br>
-- For each country, finds total number of departments situated in the country.
```sql
SELECT country_name, COUNT(department_name)
FROM countries
         LEFT JOIN locations l ON countries.country_id = l.country_id
         LEFT JOIN departments d ON l.location_id = d.location_id
GROUP BY country_name;
```


-- 5.1 d</br>
-- For each employee, finds the number of job switches of the employee.
```sql
SELECT employees.employee_id, COUNT(jh.job_id)
FROM employees
         LEFT JOIN job_history jh ON employees.employee_id = jh.employee_id
GROUP BY employees.employee_id
ORDER BY employees.employee_id;
```

-- 5.1 e</br>
/*
For each department and job types, find the total number of employees working. Print
department names, job titles, and total employees working.
 */
 ```sql
SELECT d.department_name, j.job_title, COUNT(employee_id)
FROM employees e
         LEFT JOIN jobs j
                   ON (e.job_id = j.job_id)
         JOIN departments d
              ON (e.department_id = d.department_id)
GROUP BY d.department_name, j.job_title
ORDER BY department_name;
```

-- 5.1 f</br>
/*
For each employee, finds the total number of employees those were hired before him/her.
Print employee last name and total employees.
 */
 ```sql
SELECT e1.last_name, COUNT(e2.employee_id)
FROM employees e1
         LEFT JOIN employees e2 ON (e1.hire_date > e2.hire_date)
GROUP BY e1.last_name;
```

-- 5.1 g</br>
/*
For each employee, finds the total number of employees those were hired before him/her and
those were hired after him/her. Print employee last name, total employees hired before him,
and total employees hired after him.
 */
 ```sql
SELECT LAST_NAME, "Hired before", "Hired after"
FROM (SELECT E1.EMPLOYEE_ID id, E1.LAST_NAME, COUNT(E2.EMPLOYEE_ID) "Hired before"
      FROM EMPLOYEES E1
               inner join EMPLOYEES E2
                          on (E1.HIRE_DATE > E2.HIRE_DATE)
      GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME) table1
         NATURAL JOIN
     (
         SELECT E1.EMPLOYEE_ID id, E1.LAST_NAME, COUNT(E2.EMPLOYEE_ID) "Hired after"
         FROM EMPLOYEES E1
                  inner join EMPLOYEES E2
                             on (E1.HIRE_DATE < E2.HIRE_DATE)
         GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME
     ) table2;
```

or:
```sql
WITH hired_before(last_name, hired_before)
         AS (SELECT e1.last_name, COUNT(e2.employee_id) "Hired before"
             FROM employees e1
                      LEFT JOIN employees e2 ON (e1.hire_date > e2.hire_date)
             GROUP BY e1.last_name),
     hired_after(last_name, hired_after)
         AS (
         SELECT e1.last_name, COUNT(e2.employee_id) "Hired after"
         FROM employees e1
                  LEFT JOIN employees e2 ON (e1.hire_date < e2.hire_date)
         GROUP BY e1.last_name
     )
SELECT hired_before.last_name, hired_before.hired_before, hired_after.hired_after
FROM hired_before
         INNER JOIN hired_after
                    ON (hired_before.last_name = hired_after.last_name)
ORDER BY last_name;
```

-- 5.1 h</br>
-- Find the employees having salaries greater than at least three other employees
```sql
SELECT e1.last_name, e1.salary, COUNT(e2.employee_id)
FROM employees e1
         INNER JOIN employees e2 ON e1.salary > e2.salary
GROUP BY e1.last_name, e1.salary
HAVING COUNT(e2.employee_id) >= 3
ORDER BY e1.salary;
```

-- 5.1 i</br>
```sql
SELECT E1.LAST_NAME, COUNT(DISTINCT E2.SALARY) + 1 as Rank
FROM EMPLOYEES E1
         LEFT JOIN EMPLOYEES E2
                   ON (E1.SALARY < E2.SALARY)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME
order by Rank;
```


-- 5.1 j</br>
/*
 Finds the names of employees and their salaries for the top three highest salaried employees.
The number of employees in your output should be more than three if there are employees
with same salary.
 */
 ```sql
SELECT E1.LAST_NAME, E1.SALARY
FROM EMPLOYEES E1
where (SELECT count(DISTINCT E2.SALARY)
       FROM EMPLOYEES E2
       WHERE E2.SALARY > E1.SALARY) IN (0, 1, 2);
```

<b>
  Here we are first finding the salary that are greater than E1's salary.
  If E1's salary is among the top 3 then there is only at max 2 salary could be greater than E1's salary.
  Suppose, E1's salary is the third largest. Then there will be 1st and 2nd largest.

  Here count can return 3 values :

    1. 0 - The largest salary. No salary is larger than this
    2. 1 - Second largest salary. Only one salary could be greater
    3. 2 - Third largest salary. Only two other salary could be greater.

  so, when count returns 0,1 , 2 only then we accepts the values. And print them because they are the three
  top salaries of the department.
</b>


or: 
```sql
SELECT e1.last_name, e1.salary, COUNT(DISTINCT e2.salary) + 1 AS rank
FROM employees e1
         LEFT JOIN employees e2 ON e1.salary < e2.salary
GROUP BY e1.employee_id, e1.last_name, e1.salary
HAVING COUNT(DISTINCT e2.salary) + 1 IN (1, 2, 3)
ORDER BY rank;
```
