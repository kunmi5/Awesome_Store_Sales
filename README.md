# Awesome_Store_Sales
This explores information about employees, projects, and departmental responsibilities within a financial technology (fintech) organization.
___

## Project Overview
_The organization is involved in developing and deploying various innovative solutions and services related to financial services, such as mobile payment platforms, fraud detection systems, robo-advisors, digital wallets, and regulatory compliance solutions.
The dataset contains information about employees, projects, and the departments responsible for each project within the organization. It consists of three tables: `employees`, `projects`, and `project_departments`._

---

## Tool Used:
+ SQL
### Queries
_Find the names of employees who are currently working on projects in the IT department._
```sql
select first_name, last_name, p.project_status, p.department
from employees_details as e
inner join project_details as p
on e.employee_id = p.employee_id
where project_status = 'Ongoing'
and p.department = 'IT';
```
_List the project names and the corresponding start dates for all projects that are currently ongoing._
```sql
select distinct p.project_name, start_date, project_status
from project_details as d
inner join project_departments as p
on d.project_id = p.project_id
where project_status = 'Ongoing';
```

_Retrieve the names and ages of employees who would resign after working for more than 3 years._
```sql
select first_name, last_name, age
from employees_details
where date_resigned is not null
and timestampdiff(year, date_joined, date_resigned) > 3;
```

_Find the total salary paid to employees in the 'Finance' department._
```sql
select department, sum(salary)
from employees_details
where department = 'Finance'
group by department;
```

_List the project names and employee names for projects that started in 2024._
```sql
select first_name, last_name, project_name,start_date
from ((project_details as p
inner join employees_details as e
on e.employee_id = p.employee_id)
inner join project_departments as d
on p.project_id = d.project_id)
where start_date = 2024;
```

_Find the employees who are currently working in the 'Operations' department and have a performance level of 'Exceeds'._
```sql
select first_name, last_name, department, performance_level
from employees_details 
where department = 'Operations' and performance_level = 'Exceeds';
```

_Retrieve the names of employees who joined before 2023 and are working on ongoing projects._
```sql
select first_name, last_name, date_joined, project_status
from employees_details as e
inner join project_details as p
on e.employee_id = p.employee_id
where date_joined < 2023
and project_status = 'Ongoing';
```

_Find the employees who have completed projects and are in either 'Finance' or 'IT' departments._
```sql
select first_name, last_name, p.department, project_status
from employees_details as e
inner join project_details as p
on e.employee_id = p.employee_id
where (p.department = 'Finance' or p.department ='IT')
and project_status = 'Completed';
```

_Retrieve the names of employees who share the same last name as another employee, along with their respective departments._
```sql
select distinct e1.first_name, e1.last_name, e1.department
from employees_details as e1
inner join employees_details as e2
on e1.last_name = e2.last_name
and e1.employee_id != e2.employee_id
order by e1.last_name, e1.department;
```

_Write an SQL query to find the top 3 departments with the highest average salary. Return the department and the 
average salary, rounded to 2 decimal places._
```sql
select department, round(avg(salary), 2) as avg_salary
from employees_details
group by department
order by avg_salary desc
limit 3;
```

_Write an SQL query to find the project names and the total number of employees who have joined before the
project start date. Return the project_name and the count of such employees._
```sql
select pd.project_name, count(distinct e.employee_id) as employee_count
from project_departments as pd
inner join project_details as p
on pd.project_id = p.project_id
inner join employees_details as e
on e.date_joined < p.start_date
group by pd.project_name;
```





