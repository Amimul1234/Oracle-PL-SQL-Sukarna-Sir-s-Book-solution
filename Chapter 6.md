<h1>Chapter 6: Query Multiple Tables – Sub-query</h1>
</br>
</br>


<center><h3>Solutions of chapter 6.1</h3></center>
</br>
<b>a. Find the last names of all employees that work in the SALES department.</b>

```sql
SELECT last_name
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE department_name = 'Sales'
);
```


<b>b. Find the last names and salaries of those employees who get higher salary than at least one employee of SALES department.</b>
```sql
SELECT last_name, salary
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = (
        SELECT department_id
        FROM departments
        WHERE department_name = 'Sales'
    )
);
```

<b>c. Find the last names and salaries of those employees whose salary is higher than all employees of SALES department. </b>
```sql
SELECT last_name, salary
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = (
        SELECT department_id
        FROM departments
        WHERE department_name = 'Sales'
    )
);
```


<b>d. Find the last names and salaries of those employees whose salary is within ± 5k of the average salary of SALES department.</b>
```sql
SELECT last_name, salary
FROM employees
WHERE salary BETWEEN (
                         SELECT ROUND(AVG(salary))
                         FROM employees
                         WHERE department_id = (
                             SELECT department_id
                             FROM departments
                             WHERE department_name = 'Sales'
                         )
                     ) - 5000 AND (
                                      SELECT ROUND(AVG(salary))
                                      FROM employees
                                      WHERE department_id = (
                                          SELECT department_id
                                          FROM departments
                                          WHERE department_name = 'Sales'
                                      )
                                  ) + 5000;
                                  
 ```                                 

<b>Workaround with CTE for avoiding repeated sub-query</b>
```sql
WITH sales_avg_salary
         AS (
        SELECT ROUND(AVG(salary)) avg_sal
        FROM employees
        WHERE department_id = (
            SELECT department_id
            FROM departments
            WHERE department_name = 'Sales'
        )
    )
SELECT last_name, salary
FROM employees,
     sales_avg_salary
WHERE salary BETWEEN sales_avg_salary.avg_sal - 5000 AND sales_avg_salary.avg_sal + 5000;
```
