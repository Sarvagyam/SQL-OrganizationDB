Q1: Highest Salary of employees in each department with full names.

select DeptName, CONCAT(EmpFName,' ',EmpLName) as Employee, <br />
max(Salary+Commission) as TotalSalary<br />
from employee<br />
JOIN department<br />
ON (employee.DEPTCODE = department.DEPTCODE)<br />
group by DeptName;<br />

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q1.png)



Q2: Two highest entries of departments with hiredates.

select * from (<br />
      select row_number() over(<br />
                partition by DeptName<br />
                order by HireDate) as reg,EmpCode,DeptName,HireDate<br />
      from employee e JOIN department ON (e.DEPTCODE = department.DEPTCODE)<br />
) x<br />
where x.reg <3;<br />

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q2.png)
            


Q3: First two highest salaries of employees in each department.
 
select * from (<br />
                select EmpCode,EmpFName,EmpLName,Job,(Salary+Commission),DeptName,<br />
                rank() over(partition by DeptName<br />
                            order by (Salary+Commission) DESC) as rnk<br />
                from employee e JOIN department d ON e.DEPTCODE = d.DEPTCODE) x<br />
where x.rnk <3;<br />

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q3.png)

Q4: Show Manager and employees under them and Total salary of each employee (salary + commission).

use db;<br />
select CONCAT(m.EmpFName,' ',m.EmpLName) as Manager, <br />
CONCAT(e.EmpFName,' ',e.EmpLName) as Employee,<br />
(e.Salary + e.Commission) as TotalEmpSalary from employee e <br />
JOIN employee m ON e.Manager = m.EmpCode<br />
Order by m.EmpFName;<br />
 
[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q4.png)


Q5: Details of SALESMAN hired between Jan,1980 to Dec,1990 in location MAIDSTONE.

use db;<br />
select e.*,d.DeptName,d.Location from employee e<br />
JOIN department d ON (e.DEPTCODE = d.DEPTCODE)<br />
where e.HireDate BETWEEN '1980-01-01' AND '1990-12-31'<br />
AND Location IN ('MAIDSTONE') AND e.Job IN ('SALESMAN') ;<br />

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q5.png)

Q6: Details of employees hired with empcode 9554 in dept Marketing

a.<br />
use db;<br />
select * from employee  where Extract(Year from HireDate) IN<br />
(select EXTRACT(Year from HireDate) from employee<br />
where EmpCode IN (9591,9369)) AND EmpCode NOT IN (9591,9369);<br />

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q6a.png)
 
b.
select e.* from employee e JOIN<br />
employee h ON (e.EmpCode = h.EmpCode)<br />
where EXTRACT(Year from h.Hiredate) <br />
IN (select EXTRACT(Year from Hiredate) from employee where EmpCode IN (9591,9369)) <br />
AND e.EmpCode NOT IN (9591,9369);<br />

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q6b.png)

Q7: Details of top 2 employees in each department with highest salaries(salary + commission) with descending order.

use db;<br />
select * from  (select d.DeptName,(e.Salary+e.Commission) as TotalSalary,<br />
CONCAT(e.EmpFName,' ',e.EmpLName) as EmployeeName,<br />
row_number() over(<br />
                    partition by d.DeptName<br />
                    order by (e.Salary+e.Commission) DESC) Ranks<br />
from employee e<br />
JOIN department d ON e.DEPTCODE = d.DEPTCODE) x<br />
where x.Ranks<3;<br />

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q7.png)

Q8: Minimum and maximum salary in each dept with details : employee name and empcode.

use db;<br />
select EmpCode,DeptName,CONCAT(EmpFName,' ',EmpLName) as EmployeeName,<br />
min(Salary) over(partition by DeptNAme) as MinSalary,<br />
max(Salary) over(partition by DeptName) as MaxSalary<br/>
from employee e JOIN department d<br/>
ON e.DEPTCODE = d.DEPTCODE;

[Output](https://github.com/Sarvagyam/SQL-OrganizationDB/blob/main/Q8.png)
