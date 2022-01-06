<H1>Chapter 5 practice problem solutions of Oracle PL-SQL book</H1>

-- 5.1 a</br>
-- For each employee print last name, salary, and job title.
```sql
select LAST_NAME, SALARY, JOB_TITLE
from EMPLOYEES
         inner join JOBS J on J.JOB_ID = EMPLOYEES.JOB_ID;
```

-- 5.1 b</br>
-- For each department, print department name and country name it is situated in.
```sql
select DEPARTMENT_NAME, COUNTRY_NAME
from DEPARTMENTS
         natural join LOCATIONS
         natural join COUNTRIES;
```

-- 5.1 c</br>
-- For each country, finds total number of departments situated in the country.
```sql
select COUNTRY_NAME, COUNT(DEPARTMENT_ID) "Total Number of departments"
from COUNTRIES
         natural join LOCATIONS
         natural join DEPARTMENTS
group by COUNTRY_NAME;
```


-- 5.1 d</br>
-- For each employee, finds the number of job switches of the employee.
```sql
select EMPLOYEES.EMPLOYEE_ID, count(JOB_HISTORY.JOB_ID) "Number of job switches"
FROM EMPLOYEES
         join JOB_HISTORY on EMPLOYEES.EMPLOYEE_ID = JOB_HISTORY.EMPLOYEE_ID
group by EMPLOYEES.EMPLOYEE_ID;
```

-- 5.1 e</br>
/*
For each department and job types, find the total number of employees working. Print
department names, job titles, and total employees working.
 */
 ```sql
SELECT D.DEPARTMENT_NAME, J.JOB_TITLE, count(E.EMPLOYEE_ID) "Total Number of employees"
from DEPARTMENTS D
         inner join EMPLOYEES E on D.DEPARTMENT_ID = E.DEPARTMENT_ID
         inner join JOBS J on J.JOB_ID = E.JOB_ID
group by D.DEPARTMENT_NAME, j.JOB_TITLE;
```

-- 5.1 f</br>
/*
For each employee, finds the total number of employees those were hired before him/her.
Print employee last name and total employees.
 */
 ```sql
SELECT E1.LAST_NAME, COUNT(E2.EMPLOYEE_ID) "Hired before"
FROM EMPLOYEES E1
         inner join EMPLOYEES E2
                    on (E1.HIRE_DATE > E2.HIRE_DATE)
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME;
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

-- 5.1 h</br>
-- Find the employees having salaries greater than at least three other employees
```sql
SELECT E1.EMPLOYEE_ID, E1.LAST_NAME
FROM EMPLOYEES E1
         inner join EMPLOYEES E2 on E1.SALARY > E2.SALARY
GROUP BY E1.EMPLOYEE_ID, E1.LAST_NAME
HAVING count(E2.EMPLOYEE_ID) >= 3;
```

-- 5.1 i</br>
-- TODO: Need to assign rank to salaries
SELECT
FROM EMPLOYEES E1
         inner join EMPLOYEES E2 ON E1.SALARY



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
