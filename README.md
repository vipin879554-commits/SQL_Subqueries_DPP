Employees(emp_id, name, department_id, salary)
Departments(department_id, department_name, location)
Sales(sale_id, emp_id, sale_amount, sale_date)
## BASIC LEVEL
# Question 1: Retrieve the names of employees who earn more than the average salary of all employees.
sql
Copy code
SELECT name
FROM Employees
WHERE salary > (SELECT AVG(salary) FROM Employees);
# Question 2: Find the employees who belong to the department with the highest average salary.
sql
Copy code
SELECT e.name
FROM Employees e
WHERE e.department_id = (
    SELECT department_id
    FROM Employees
    GROUP BY department_id
    ORDER BY AVG(salary) DESC
    LIMIT 1
);
# Question 3: List all employees who have made at least one sale.
sql
Copy code
SELECT name
FROM Employees
WHERE emp_id IN (SELECT emp_id FROM Sales);
# Question 4: Find the employee with the highest sale amount.
sql
Copy code
SELECT name
FROM Employees
WHERE emp_id = (
    SELECT emp_id
    FROM Sales
    ORDER BY sale_amount DESC
    LIMIT 1
);
# Question 5: Retrieve the names of employees whose salaries are higher than Shubham’s salary.
sql
Copy code
SELECT name
FROM Employees
WHERE salary > (
    SELECT salary FROM Employees WHERE name = 'Shubham'
);
## INTERMEDIATE LEVEL
# Question 1: Find employees who work in the same department as Abhishek.
sql
Copy code
SELECT name
FROM Employees
WHERE department_id = (
    SELECT department_id
    FROM Employees
    WHERE name = 'Abhishek'
);
# Question 2: List departments that have at least one employee earning more than ₹60,000.
sql
Copy code
SELECT department_name
FROM Departments
WHERE department_id IN (
    SELECT department_id
    FROM Employees
    WHERE salary > 60000
);
# Question 3: Find the department name of the employee who made the highest sale.
sql
Copy code
SELECT d.department_name
FROM Departments d
WHERE d.department_id = (
    SELECT e.department_id
    FROM Employees e
    WHERE e.emp_id = (
        SELECT emp_id
        FROM Sales
        ORDER BY sale_amount DESC
        LIMIT 1
    )
);
# Question 4: Retrieve employees who have made sales greater than the average sale amount.
sql
Copy code
SELECT name
FROM Employees
WHERE emp_id IN (
    SELECT emp_id
    FROM Sales
    WHERE sale_amount > (SELECT AVG(sale_amount) FROM Sales)
);
# Question 5: Find the total sales made by employees who earn more than the average salary.
sql
Copy code
SELECT SUM(sale_amount) AS TotalSales
FROM Sales
WHERE emp_id IN (
    SELECT emp_id
    FROM Employees
    WHERE salary > (SELECT AVG(salary) FROM Employees)
);
## ADVANCED LEVEL
# Question 1: Find employees who have not made any sales.
sql
Copy code
SELECT name
FROM Employees
WHERE emp_id NOT IN (SELECT emp_id FROM Sales);
# Question 2: List departments where the average salary is above ₹55,000.
sql
Copy code
SELECT department_name
FROM Departments
WHERE department_id IN (
    SELECT department_id
    FROM Employees
    GROUP BY department_id
    HAVING AVG(salary) > 55000
);
# Question 3: Retrieve department names where the total sales exceed ₹10,000.
sql
Copy code
SELECT d.department_name
FROM Departments d
WHERE d.department_id IN (
    SELECT e.department_id
    FROM Employees e
    WHERE e.emp_id IN (
        SELECT emp_id
        FROM Sales
        GROUP BY emp_id
        HAVING SUM(sale_amount) > 10000
    )
);
# Question 4: Find the employee who has made the second-highest sale.
sql
Copy code
SELECT name
FROM Employees
WHERE emp_id = (
    SELECT emp_id
    FROM Sales
    ORDER BY sale_amount DESC
    LIMIT 1 OFFSET 1
);
# Question 5: Retrieve the names of employees whose salary is greater than the highest sale amount recorded.
sql
Copy code
SELECT name
FROM Employees
WHERE salary > (SELECT MAX(sale_amount) FROM Sales);
