<h3><b>Practice problems 3.3</b></h3>

<h3>a) For all employees, find the number of years employed. Print first names and number of years
employed for each employee. </h3>

</br>

```sql
SELECT employee_id, last_name, TRUNC(MONTHS_BETWEEN(SYSDATE, hire_date) / 12) "Number of years employeed"
FROM employees;
```

<h3>
b) Suppose you need to find the number of days each employee worked during the first month  of his joining. Write an SQL query to find this information for all employees.
</h3>
</br>

```sql
SELECT employee_id,
       last_name,
       (TRUNC(ADD_MONTHS(hire_date, 1), 'MONTH') - hire_date - 1) "Works during first month of joining"
FROM employees;
```

<h3><u>Solution thought:</u><h3>
<ul>
    <li>Here first we are finding the start of the next month by doing TRUNC(ADD_MONTHS(hire_date, 1), 'MONTH')
    <li>Once we got the next month start now work is pretty simple.
    <li>Just do a minus of next month start and his hire date
    <li> -1 is for eliminating next month start from result. We need only the hiring month calculation.
</ul>