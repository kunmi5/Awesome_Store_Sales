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
#### Find the names of employees who are currently working on projects in the IT department.

+ Insight: _This helps isolate human resources contributing to IT deliverables in real time, useful for tracking workloads or evaluating ongoing resource allocation._
```sql
select first_name, last_name, p.project_status, p.department
from employees_details as e
inner join project_details as p
on e.employee_id = p.employee_id
where project_status = 'Ongoing'
and p.department = 'IT';
```
#### List the project names and the corresponding start dates for all projects that are currently ongoing.

+ Insight: _Offers visibility into active projects and their initiation timelines—valuable for understanding project duration and active workload._
```sql
select distinct p.project_name, start_date, project_status
from project_details as d
inner join project_departments as p
on d.project_id = p.project_id
where project_status = 'Ongoing';
```

#### Retrieve the names and ages of employees who would resign after working for more than 3 years.

+ Insight: _Identifies long-tenure employees who left, which could inform retention analysis or exit interview focus._
```sql
select first_name, last_name, age
from employees_details
where date_resigned is not null
and timestampdiff(year, date_joined, date_resigned) > 3;
```

#### Find the total salary paid to employees in the 'Finance' department.

+ Insight: _Useful for departmental budget tracking and financial forecasting._
```sql
select department, sum(salary)
from employees_details
where department = 'Finance'
group by department;
```

#### List the project names and employee names for projects that started in 2024.

+ Insight: _Helps in understanding recent project launches and staffing patterns in the current year._
```sql
select first_name, last_name, project_name,start_date
from ((project_details as p
inner join employees_details as e
on e.employee_id = p.employee_id)
inner join project_departments as d
on p.project_id = d.project_id)
where start_date = 2024;
```

#### Find the employees who are currently working in the 'Operations' department and have a performance level of 'Exceeds'.

+ Insight: _Highlights top performers in a key department, valuable for promotions, bonuses, or leadership grooming._
```sql
select first_name, last_name, department, performance_level
from employees_details 
where department = 'Operations' and performance_level = 'Exceeds';
```

#### Retrieve the names of employees who joined before 2023 and are working on ongoing projects.

+ Insight: _Helps assess experience level on ongoing projects—possibly correlating tenure with performance or project continuity._
```sql
select first_name, last_name, date_joined, project_status
from employees_details as e
inner join project_details as p
on e.employee_id = p.employee_id
where date_joined < 2023
and project_status = 'Ongoing';
```

#### Find the employees who have completed projects and are in either 'Finance' or 'IT' departments.

+ Insight: _Identifies personnel with full project lifecycle experience in strategic departments—useful for reassignment or recognition._
```sql
select first_name, last_name, p.department, project_status
from employees_details as e
inner join project_details as p
on e.employee_id = p.employee_id
where (p.department = 'Finance' or p.department ='IT')
and project_status = 'Completed';
```

#### Retrieve the names of employees who share the same last name as another employee, along with their respective departments.

+ Insight: _While seemingly trivial, this may help identify familial ties or just handle data ambiguity in HR systems._
```sql
select distinct e1.first_name, e1.last_name, e1.department
from employees_details as e1
inner join employees_details as e2
on e1.last_name = e2.last_name
and e1.employee_id != e2.employee_id
order by e1.last_name, e1.department;
```

#### Write an SQL query to find the top 3 departments with the highest average salary. Return the department and the 
#### average salary, rounded to 2 decimal places.

+ Insight: _Provides a snapshot of compensation hierarchy across departments—can signal high-skill or high-demand areas._
```sql
select department, round(avg(salary), 2) as avg_salary
from employees_details
group by department
order by avg_salary desc
limit 3;
```

#### Write an SQL query to find the project names and the total number of employees who have joined before the
#### project start date. Return the project_name and the count of such employees.

+ Insight: _Helps assess how many resources were already in place before project initiation—indicative of staffing readiness or onboarding gaps._
```sql
select pd.project_name, count(distinct e.employee_id) as employee_count
from project_departments as pd
inner join project_details as p
on pd.project_id = p.project_id
inner join employees_details as e
on e.date_joined < p.start_date
group by pd.project_name;
```

### Screenshots

<img width="369" height="351" alt="Screenshot 2025-08-04 214710" src="https://github.com/user-attachments/assets/7d023e62-5888-46f4-934a-39c954257ba8" />

<img width="369" height="337" alt="Screenshot 2025-08-04 214754" src="https://github.com/user-attachments/assets/91e7ba07-91c8-4a4c-bd94-680c5e92a334" />

<img width="369" height="344" alt="Screenshot 2025-08-04 214844" src="https://github.com/user-attachments/assets/3fded015-3519-4e7d-b8e7-268664c8ebcb" />

### 📌 General Observation & Recommendation

Analyzing the SQL queries revealed valuable patterns in employee engagement, departmental performance, and project timelines. 
The data shows a consistent allocation of employees to ongoing projects—especially in departments like IT and Finance—while also highlighting high-performing individuals and long-tenured employees who have recently resigned. Additionally, salary disparities across departments and performance variations suggest areas for optimization in workforce management and compensation strategy.

To improve overall organizational efficiency, it is recommended to establish regular monitoring of project progress, employee performance, and departmental spending. Recognizing top performers, understanding resignation trends, and ensuring fair compensation structures can lead to better talent retention and more effective project execution. Leveraging data-driven insights like these can significantly enhance HR and operational decision-making.







