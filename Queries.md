Q1: Highest Salary of employees in each department with full names.
````sql
select DeptName, CONCAT(EmpFName,' ',EmpLName) as Employee,
max(Salary+Commission) as TotalSalary
from employee
JOIN department
ON (employee.DEPTCODE = department.DEPTCODE)
group by DeptName;
````

<img src="https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q1.png" height="30px" width="120px" >



Q2: Two highest entries of departments with hiredates.
````sql
select * from (
      select row_number() over(
                partition by DeptName
                order by HireDate) as reg,EmpCode,DeptName,HireDate
      from employee e JOIN department ON (e.DEPTCODE = department.DEPTCODE)
) x
where x.reg <3;
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q2.png)
            


Q3: First two highest salaries of employees in each department.
````sql
select * from (
                select EmpCode,EmpFName,EmpLName,Job,(Salary+Commission),DeptName,
                rank() over(partition by DeptName
                            order by (Salary+Commission) DESC) as rnk
                from employee e JOIN department d ON e.DEPTCODE = d.DEPTCODE) x
where x.rnk <3;
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q3.png)

Q4: Show Manager and employees under them and Total salary of each employee (salary + commission).
````sql
use db;
select CONCAT(m.EmpFName,' ',m.EmpLName) as Manager, 
CONCAT(e.EmpFName,' ',e.EmpLName) as Employee,
(e.Salary + e.Commission) as TotalEmpSalary from employee e 
JOIN employee m ON e.Manager = m.EmpCode
Order by m.EmpFName;
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q4.png)


Q5: Details of SALESMAN hired between Jan,1980 to Dec,1990 in location MAIDSTONE.
````sql
use db;<br />
select e.*,d.DeptName,d.Location from employee e<br />
JOIN department d ON (e.DEPTCODE = d.DEPTCODE)<br />
where e.HireDate BETWEEN '1980-01-01' AND '1990-12-31'<br />
AND Location IN ('MAIDSTONE') AND e.Job IN ('SALESMAN') ;<br />
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q5.png)

Q6: Details of employees hired with empcode 9554 in dept Marketing

a.
````sql
use db;
select * from employee  where Extract(Year from HireDate) IN
(select EXTRACT(Year from HireDate) from employee
where EmpCode IN (9591,9369)) AND EmpCode NOT IN (9591,9369);
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q6a.png)
 
b.
````sql
select e.* from employee e JOIN
employee h ON (e.EmpCode = h.EmpCode)
where EXTRACT(Year from h.Hiredate) 
IN (select EXTRACT(Year from Hiredate) from employee where EmpCode IN (9591,9369))
AND e.EmpCode NOT IN (9591,9369);
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q6b.png)

Q7: Details of top 2 employees in each department with highest salaries(salary + commission) with descending order.
````sql
use db;
select * from  (select d.DeptName,(e.Salary+e.Commission) as TotalSalary,
CONCAT(e.EmpFName,' ',e.EmpLName) as EmployeeName,
row_number() over(
                    partition by d.DeptName
                    order by (e.Salary+e.Commission) DESC) Ranks
from employee e
JOIN department d ON e.DEPTCODE = d.DEPTCODE) x
where x.Ranks<3;
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q7.png)

Q8: Minimum and maximum salary in each dept with details : employee name and empcode.
````sql
use db;
select EmpCode,DeptName,CONCAT(EmpFName,' ',EmpLName) as EmployeeName,
min(Salary) over(partition by DeptNAme) as MinSalary,
max(Salary) over(partition by DeptName) as MaxSalary
from employee e JOIN department d
ON e.DEPTCODE = d.DEPTCODE;
````
![Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q8.png)
