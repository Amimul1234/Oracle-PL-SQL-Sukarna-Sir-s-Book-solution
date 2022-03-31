<h3><b>Practice problems 4.1</b></h3>
</br>

<h4>
    a) For all managers, find the number of employees he/she manages. Print the MANAGER_ID
    and total number of such employees.
</h4>

```sql
    SELECT manager_id, COUNT(employee_id) "Number of employees"
    FROM employees
    GROUP BY manager_id;
```

<h4>
    b) For all departments, find the number of employees who get more than 30k salary. Print the DEPARTMENT_ID and total number of such employees.
</h4>

```sql
    SELECT department_id, COUNT(employee_id)
    FROM employees
    WHERE salary > 30000
    GROUP BY department_id;
```

<h4>
    c) Find the minimum, maximum, and average salary of all departments except
    DEPARTMENT_ID 80. Print DEPARTMENT_ID, minimum, maximum, and average salary.
    Sort the results in descending order of average salary first, then maximum salary, then
    minimum salary. Use column alias to rename column names in output for better display.For all departments, find the number of employees who get more than 30k salary. Print the DEPARTMENT_ID and total number of such employees.
</h4>

```sql
    SELECT department_id, MIN(salary), MAX(salary), ROUND(AVG(salary), 2)
    FROM employees
    WHERE department_id != 80
    GROUP BY department_id
    ORDER BY ROUND(AVG(salary), 2) DESC, MAX(salary) DESC, MIN(salary) DESC;
```

</br>

<h3><b>Practice problems 4.2</b></h3>
</br>

<h4>
    a) Find for each department, the average salary of the department. Print only those
    DEPARTMENT_ID and average salary whose average salary is at most 50k
</h4>

```sql
    SELECT department_id, AVG(salary)
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary) <= 50000;
```


<h3><b>Practice problems 4.3</b></h3>
</br>

<h4>
    a) Find number of employees in each salary group. Salary groups are considered as follows. Group 1: 0k to &#60 5K, 5k to &#60 10k, 10k to &#60 15k, and so on. 
</h4>

```sql
    SELECT (TRUNC(salary / 5000, 0) + 1) "Salary Group", COUNT(*) "Number of employees"
    FROM employees
    GROUP BY TRUNC(salary / 5000, 0) + 1
    ORDER BY "Salary Group";
```

<h4>b) Find the number of employees that were hired in each year in each job type. Print year, job id,  and total employees hired.</h4>

```sql
    SELECT TO_CHAR(hire_date, 'YYYY'), job_id, COUNT(employee_id)
    FROM employees
    GROUP BY TO_CHAR(hire_date, 'YYYY'), job_id
    ORDER BY TO_CHAR(hire_date, 'YYYY');
```