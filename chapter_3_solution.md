<h3><b>Practice problems 3.1</b></h3>
</br>

<h4>a) Print the first three characters and last three characters of all country names. Print in capital
letters </h4>

```sql
    SELECT UPPER(SUBSTR(country_name, 0, 3)) || ' ' || UPPER(SUBSTR(country_name, LENGTH(country_name) - 2, 3))
    FROM countries;
```

<h4>b) Print all employee full names (first name followed by a space then followed by last name).
All names should be printed in width of 60 characters and left padded with '*' symbol for
names less than 60 characters.</h4>

```sql
    SELECT LPAD((FIRST_NAME || ' ' || LAST_NAME), 60, '*') FULLNAME
    from employees;
```

<h4>c) Print all job titles that contain the text 'manager'</h4>

```sql
    SELECT job_id
    FROM employees
    WHERE LOWER(job_id) = '%manager%';
```

<h3><b>Practice problems 3.2</b></h3>
</br>

<h4>a) Print employee last name and number of days employed. Print the second information
rounded up to 2 decimal places.</h4>

```sql
    SELECT last_name, ROUND((SYSDATE - hire_date), 2) "Number of days employeed"
    FROM employees;
```

<h4>b) Print employee last name and number of years employed. Print the second information
truncated up to 3 decimal place.</h4>

```sql
    SELECT last_name, TRUNC((SYSDATE - hire_date) / 365, 3) "Number of years employeed"
    FROM employees;
```

<h3><b>Practice problems 3.3</b></h3>
</br>

<h4>a) For all employees, find the number of years employed. Print first names and number of years
employed for each employee. </h4>

</br>

```sql
SELECT employee_id, last_name, TRUNC(MONTHS_BETWEEN(SYSDATE, hire_date) / 12) "Number of years employeed"
FROM employees;
```

<h4>
b) Suppose you need to find the number of days each employee worked during the first month  of his joining. Write an SQL query to find this information for all employees.
</h4>
</br>

```sql
SELECT employee_id,
       last_name,
       (TRUNC(ADD_MONTHS(hire_date, 1), 'MONTH') - hire_date - 1) "Works during first month of joining"
FROM employees;
```

<h4><u>Solution thought:</u><h4>
<ul>
    <li>Here first we are finding the start of the next month by doing TRUNC(ADD_MONTHS(hire_date, 1), 'MONTH')
    <li>Once we got the next month start now work is pretty simple.
    <li>Just do a minus of next month start and his hire date
    <li> -1 is for eliminating next month start from result. We need only the hiring month calculation.
</ul>