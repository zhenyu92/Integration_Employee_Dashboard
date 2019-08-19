# A study on a company employee database with the Integration of SQL & Tableau 2019.
This project is a part of the learning milestone of a Udemy course delivered by [365 Careers](https://www.udemy.com/the-business-intelligence-analyst-course-2018/). 

### Author
[Derrick Chan](https://github.com/zhenyu92)

[Derrick's Linkedin](https://www.linkedin.com/in/zychan/)

### Project Status: [Completed]

### Project Objective
A database contains various employee information was given. The database contains the following datasets:
- t_departments
- t_dept_emp
- t_dept_manager
- t_employees
- t-salaries

The relational schema is as follows:
![alt text](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/employees-mod-db-1.jpg "Relational Schema")

We are required to retrive information from the database and present the following findings in a Tableau Dashboard:
- Task 1: Create a visualization that provides a breakdown between the male and female employees working in the company each year, starting from 1990.
- Task 2: Compare the number of male managers to the number of female managers from different departments for each year, starting from 1990.
- Task 3: Compare the average salary of female versus male employees in the entire company until year 2002, and add a filter allowing you to see that per each department.
- Task 4: Create an SQL stored procedure that will allow you to obtain the average male and female salary per department within a certain salary range. Let this range be defined by two values the user can insert when calling the procedure. Finally, visualize the obtained result-set in Tableau as a double bar chart.

### Environment Prerequisites
`MySQL` for database query
`Tableau 2019` for data visualization

### Instructions
1. Downloaded the [database](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/employees_mod.zip).
2. Run and execute the `SQL` file in MySQL to build the database.
3. For Task 1, run and execute the [SQL Script](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_1.sql).
    ```
    SELECT 
      YEAR(d.from_date) AS calendar_year,
      e.gender,
      COUNT(e.emp_no) AS num_of_employees
      
    FROM
      t_employees e
        JOIN
      t_dept_emp d ON d.emp_no = e.emp_no
      
    GROUP BY calendar_year , e.gender
    
    HAVING calendar_year >= 1990
    
    ORDER BY 1;
    ```
4. Export the query of Task 1 as [CSV File](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_1.csv)
5. For Task 2, run and execute the [SQL Script](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_2.sql).
    ```
    SELECT 
      d.dept_name,
      ee.gender,
      dm.emp_no,
      dm.from_date,
      dm.to_date,
      e.calendar_year,
      CASE
        WHEN YEAR(dm.to_date) >= e.calendar_year AND YEAR(dm.from_date) <= e.calendar_year THEN 1
        ELSE 0
      END AS active
      
    FROM
      (SELECT
        YEAR(hire_date) AS calendar_year
      FROM
        t_employees
      GROUP BY 
        calendar_year) e
          CROSS JOIN
            t_dept_manager dm
          JOIN
            t_departments d ON dm.dept_no = d.dept_no
          JOIN 
            t_employees ee ON dm.emp_no = ee.emp_no
            
      ORDER BY dm.emp_no, calendar_year;
    ```
6. Export the query of Task 2 as [CSV File](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_2.csv)
7. For Task 3, run and execute the [SQL Script](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_3.sql).
    ```
    SELECT 
      e.gender,
      d.dept_name,
      ROUND(AVG(s.salary), 2) AS salary,
      YEAR(s.from_date) AS calendar_year
    FROM
      t_salaries s
        JOIN
      t_employees e ON s.emp_no = e.emp_no
        JOIN
    t_dept_emp de ON de.emp_no = e.emp_no
        JOIN
    t_departments d ON d.dept_no = de.dept_no
    
  GROUP BY d.dept_no , e.gender , calendar_year

  HAVING calendar_year <= 2002

  ORDER BY d.dept_no;
    ```
8. Export the query of Task 3 as [CSV File](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_3.csv)
9. For Task 4, run and execute the [SQL Script](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_4.sql).
    ```
    DROP PROCEDURE IF EXISTS filter_salary;
    DELIMITER $$
    CREATE PROCEDURE filter_salary (IN p_min_salary FLOAT, IN p_max_salary FLOAT)
    BEGIN
    
    SELECT 
      e.gender, 
      d.dept_name, 
      AVG(s.salary) as avg_salary
      
    FROM
      t_salaries s
        JOIN
      t_employees e ON s.emp_no = e.emp_no
        JOIN
      t_dept_emp de ON de.emp_no = e.emp_no
        JOIN
      t_departments d ON d.dept_no = de.dept_no
    
    WHERE s.salary BETWEEN p_min_salary AND p_max_salary

    GROUP BY d.dept_no, e.gender;
    
    END$$
    DELIMITER ;
    CALL filter_salary(50000, 90000);
    ```
10. Export the query of Task 4 as [CSV File](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_4.csv)
11. Import and tuning in Tableau 2019.
- Task 1
![alt text](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_1.JPG "Task 1")
- Task 2
![alt text](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_1.JPG "Task 2")
- Task 3
![alt text](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_1.JPG "Task 3")
- Task 4
![alt text](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Task_1.JPG "Task 4")

### Final Tableau Dashboard
Tableau workbook for this project can be viewed and downloaded from [Here](https://public.tableau.com/profile/derrick1466#!/vizhome/AnEmployeeDatasetDashboard/Dashboard1?publish=yes).

### Screenshot of the Dashboard:
![alt text](https://github.com/zhenyu92/Tableau-SQL_Employee_Dashboard/blob/master/Dashboard%201.png "Final Dashboard")
