# 解决 SQL:MySQL 中的 SQL 练习和解决方案

> 原文：<https://medium.com/mlearning-ai/solve-sql-sql-exercises-and-solutions-in-mysql-a099fcbfc25e?source=collection_archive---------0----------------------->

最近引起我注意的一个表达是:

> *数据是新的石油。*

我不确定是谁第一次使用了这个术语，也不知道谁应该为此得到荣誉。但是，引用得太恰当了！挖掘或提炼是数据的重要组成部分。将原始数据转化为一些有用的见解是一些有效商业战略的关键。在这整个过程中，SQL 扮演着重要的角色。所以，这篇文章是关于数据和 SQL 的。

有时候，我喜欢用 SQL 挑战自己。T2 哈克兰克是一个非常好的地方。但是，一旦简单和中等的挑战结束，我只剩下复杂的挑战，我明白我需要更经常地学习和练习 SQL。下一个挑战是我可以在哪里练习 SQL？有几个 SQL 平台可供选择，我喜欢 [db-fiddle](https://www.db-fiddle.com/) 。我使用免费版本，我看不到任何选项来构建我自己的模式并重用它。尽管如此，它仍然是创建表、编写查询和练习 SQL 的好地方。这篇文章是关于 SQL 的练习，为面试做准备，或者只是为了测试你自己的 SQLMETER 水平。

所以，首先，让我们从头开始创建一些表格并插入一些数据，然后我们将做一些真正的练习。

员工和部门是解决和实践许多数据问题的一个非常常见的例子。也许创建一个表并插入记录太容易了，但这是一项令人厌倦的工作。因此，这里我提供了一个 SQL 脚本来创建表和插入记录，它将在 dB-fiddle MySQL 5.7 版中工作。

**创建一个带有主键列的表**

```
CREATE TABLE departments (department_id INTEGER PRIMARY KEY ,       
                          department_name VARCHAR(30) , 
                          location_id INTEGER ) ;
```

**创建一个带有外键的表**

```
CREATE TABLE employees ( employee_id INTEGER , 
                         first_name VARCHAR(20) , 
                         last_name VARCHAR(25) , 
                         email VARCHAR(25) , 
                         phone_number VARCHAR(20) , 
                         hire_date DATE , 
                         job_id VARCHAR(10) , 
                         salary INTEGER , 
                         commission_pct INTEGER , 
                         manager_id INTEGER , 
                         department_id INTEGER , 
constraint pk_emp primary key (employee_id) , 
constraint fk_deptno foreign key (department_id) references departments(department_id) ) ;
```

**将记录插入表格**

```
## Insert insto Departments table INSERT INTO departments VALUES ( 20,'Marketing', 180); INSERT INTO departments VALUES ( 30,'Purchasing', 1700); 
INSERT INTO departments VALUES ( 40, 'Human Resources', 2400); INSERT INTO departments VALUES ( 50, 'Shipping', 1500); 
INSERT INTO departments VALUES ( 60 , 'IT', 1400); 
INSERT INTO departments VALUES ( 70, 'Public Relations', 2700); INSERT INTO departments VALUES ( 80 , 'Sales', 2500 ); 
INSERT INTO departments VALUES ( 90 , 'Executive', 1700); 
INSERT INTO departments VALUES ( 100 , 'Finance', 1700); 
INSERT INTO departments VALUES ( 110 , 'Accounting', 1700); 
INSERT INTO departments VALUES ( 120 , 'Treasury' , 1700); 
INSERT INTO departments VALUES ( 130 , 'Corporate Tax' , 1700 ); INSERT INTO departments VALUES ( 140, 'Control And Credit' , 1700); INSERT INTO departments VALUES ( 150 ,'Shareholder Services', 1700); 
INSERT INTO departments VALUES ( 160 , 'Benefits', 1700); 
INSERT INTO departments VALUES ( 170 , 'Payroll' , 1700); ## Insert into Employees table 
INSERT INTO employees VALUES (100, 'Steven', 'King', 'SKING', '515.123.4567', '1987-06-17' , 'AD_PRES', 24000 , NULL, NULL, 20); INSERT INTO employees VALUES (101, 'Neena' , 'Kochhar' , 'NKOCHHAR' , '515.123.4568' , '1989-11-21' , 'AD_VP' , 17000 , NULL , 100 , 20); 
INSERT INTO employees VALUES (102 , 'Lex' , 'De Haan' , 'LDEHAAN' , '515.123.4569' , '1993-09-12' , 'AD_VP' , 17000 , NULL , 100 , 30); INSERT INTO employees VALUES (103 , 'Alexander' , 'Hunold' , 'AHUNOLD' , '590.423.4567' , '1990-09-30', 'IT_PROG' , 9000 , NULL , 102 , 60); 
INSERT INTO employees VALUES (104 , 'Bruce' , 'Ernst' , 'BERNST' , '590.423.4568' , '1991-05-21', 'IT_PROG' , 6000 , NULL , 103 , 60); INSERT INTO employees VALUES (105 , 'David' , 'Austin' , 'DAUSTIN' , '590.423.4569' , '1997-06-25', 'IT_PROG' , 4800 , NULL , 103 , 60); INSERT INTO employees VALUES (106 , 'Valli' , 'Pataballa' , 'VPATABAL' , '590.423.4560' , '1998-02-05', 'IT_PROG' , 4800 , NULL , 103 , 40); 
INSERT INTO employees VALUES (107 , 'Diana' , 'Lorentz' , 'DLORENTZ' , '590.423.5567' , '1999-02-09', 'IT_PROG' , 4200 , NULL , 103 , 40); 
INSERT INTO employees VALUES (108 , 'Nancy' , 'Greenberg' , 'NGREENBE' , '515.124.4569' , '1994-08-17', 'FI_MGR' , 12000 , NULL , 101 , 100); 
INSERT INTO employees VALUES (109 , 'Daniel' , 'Faviet' , 'DFAVIET' , '515.124.4169' , '1994-08-12', 'FI_ACCOUNT' , 9000 , NULL , 108 , 170); 
INSERT INTO employees VALUES (110 , 'John' , 'Chen' , 'JCHEN' , '515.124.4269' , '1997-04-09', 'FI_ACCOUNT' , 8200 , NULL , 108 , 170); 
INSERT INTO employees VALUES (111 , 'Ismael' , 'Sciarra' , 'ISCIARRA' , '515.124.4369' , '1997-02-01', 'FI_ACCOUNT' , 7700 , NULL , 108 , 160); 
INSERT INTO employees VALUES (112 , 'Jose Manuel' , 'Urman' , 'JMURMAN' , '515.124.4469' , '1998-06-03', 'FI_ACCOUNT' , 7800 , NULL , 108 , 150); 
INSERT INTO employees VALUES (113 , 'Luis' , 'Popp' , 'LPOPP' , '515.124.4567' , '1999-12-07', 'FI_ACCOUNT' , 6900 , NULL , 108 , 140); 
INSERT INTO employees VALUES (114 , 'Den' , 'Raphaely' , 'DRAPHEAL' , '515.127.4561' , '1994-11-08', 'PU_MAN' , 11000 , NULL , 100 , 30); 
INSERT INTO employees VALUES (115 , 'Alexander' , 'Khoo' , 'AKHOO' , '515.127.4562' , '1995-05-12', 'PU_CLERK' , 3100 , NULL , 114 , 80); INSERT INTO employees VALUES (116 , 'Shelli' , 'Baida' , 'SBAIDA' , '515.127.4563' ,'1997-12-13', 'PU_CLERK' , 2900 , NULL , 114 , 70); INSERT INTO employees VALUES (117 , 'Sigal' , 'Tobias' , 'STOBIAS' , '515.127.4564' , '1997-09-10', 'PU_CLERK' , 2800 , NULL , 114 , 30); INSERT INTO employees VALUES (118 , 'Guy' , 'Himuro' , 'GHIMURO' , '515.127.4565' , '1998-01-02', 'PU_CLERK' , 2600 , NULL , 114 , 60); INSERT INTO employees VALUES (119 , 'Karen' , 'Colmenares' , 'KCOLMENA' , '515.127.4566' , '1999-04-08', 'PU_CLERK' , 2500 , NULL , 114 , 130); 
INSERT INTO employees VALUES (120 , 'Matthew' , 'Weiss' , 'MWEISS' , '650.123.1234' ,'1996-07-18', 'ST_MAN' , 8000 , NULL , 100 , 50); INSERT INTO employees VALUES (121 , 'Adam' , 'Fripp' , 'AFRIPP' , '650.123.2234' , '1997-08-09', 'ST_MAN' , 8200 , NULL , 100 , 50); INSERT INTO employees VALUES (122 , 'Payam' , 'Kaufling' , 'PKAUFLIN' , '650.123.3234' ,'1995-05-01', 'ST_MAN' , 7900 , NULL , 100 , 40); 
INSERT INTO employees VALUES (123 , 'Shanta' , 'Vollman' , 'SVOLLMAN' , '650.123.4234' , '1997-10-12', 'ST_MAN' , 6500 , NULL , 100 , 50); 
INSERT INTO employees VALUES (124, 'Kevin' , 'Mourgos' , 'KMOURGOS' , '650.123.5234' , '1999-11-12', 'ST_MAN' , 5800 , NULL , 100 , 80); INSERT INTO employees VALUES (125, 'Julia' , 'Nayer' , 'JNAYER' , '650.124.1214' , '1997-07-02', 'ST_CLERK' , 3200 , NULL , 120 , 50); INSERT INTO employees VALUES (126, 'Irene' , 'Mikkilineni' , 'IMIKKILI' , '650.124.1224' , '1998-11-12', 'ST_CLERK' , 2700 , NULL , 120 , 50); 
INSERT INTO employees VALUES (127, 'James' , 'Landry' , 'JLANDRY' , '650.124.1334' , '1999-01-02' , 'ST_CLERK' , 2400 , NULL , 120 , 90); 
INSERT INTO employees VALUES (128, 'Steven' , 'Markle' , 'SMARKLE' , '650.124.1434' , '2000-03-04' , 'ST_CLERK' , 2200 , NULL , 120 , 50); 
INSERT INTO employees VALUES (129, 'Laura' , 'Bissot' , 'LBISSOT' , '650.124.5234' ,'1997-09-10' , 'ST_CLERK' , 3300 , NULL , 121 , 50); INSERT INTO employees VALUES (130, 'Mozhe' , 'Atkinson' , 'MATKINSO' , '650.124.6234' , '1997-10-12' , 'ST_CLERK' , 2800 , NULL , 121 , 110);
```

因此，现在我们有 2 个表和一些数据准备运行我们的 SQL。该做些练习了。

**1。选择名字以字母 S 开头的雇员的名字、姓氏、职务 id 和薪金**

```
select first_name, 
       last_name, job_id, 
       salary 
from employees 
where upper(first_name) like 'S%';
```

**2。编写一个查询来选择薪水最高的雇员**

```
select employee_id, 
first_name, 
last_name, 
job_id, 
salary 
from employees 
where salary = (select max(salary) from employees);
```

**3。选择薪资第二高的员工**

```
select employee_id, 
first_name, 
last_name, 
job_id, 
salary 
from employees 
where salary != (select max(salary) from employees) 
order by salary desc 
limit 1;
```

上面的查询只选择了一个薪水第二高的人。但是如果有 1 个以上的人工资一样呢？或者，如果我们想选择第三或第四高的薪水呢？所以，让我们尝试一种通用的方法。

**4。获取工资第二或第三高的员工**

```
#change the input for 2nd, 3rd or 4th highest salary set 
@input:=3; 
select employee_id, 
first_name, 
last_name, 
job_id, 
salary 
from employees e 
where @input =(select COUNT(DISTINCT Salary) 
               from employees p 
               where e.Salary<=p.Salary);
```

**5。编写一个查询来选择雇员和他们相应的经理以及他们的工资**

现在，这是 SQL 练习中**自连接**的一个经典例子。此外，我使用 **CONCAT** 函数连接每个雇员和经理的名和姓。

```
select concat(emp.first_name,' ',emp.last_name) employee, 
emp.salary emp_sal, 
concat(mgr.first_name,' ',mgr.last_name) manager, 
mgr.salary mgr_sal 
from employees emp 
join employees mgr on emp.manager_id = mgr.employee_id;
```

**6。编写一个查询，以降序显示每个经理手下的员工数量**

```
select sup.employee_id employee_id, 
concat(sup.first_name,' ', sup.last_name)manager_name, 
COUNT (sub.employee_id) AS number_of_reportees 
from employees sub 
join employees sup on sub.manager_id = sup.employee_id 
group by sup.employee_id, sup.first_name, sup.last_name 
order by 3 desc;
```

**7。找出每个部门的员工人数**

```
select dept.department_name, 
count(emp.employee_id) emp_count 
from employees emp 
join departments dept on emp.department_id = dept.department_id group by dept.department_name 
order by 2 desc;
```

**8。获得每年雇佣的员工数量**

```
select year(hire_date) hired_year, 
count(*) employees_hired_count 
from employees 
group by year(hire_date) 
order by 2 desc;
```

**9。查找员工的薪资范围**

```
select min(salary) min_sal, 
max(salary) max_sal, 
round(avg(salary)) avg_sal 
from employees;
```

**10。编写一个查询，根据薪水将人们分成三组**

```
select concat(first_name,' ',last_name) employee, salary, 
case 
when salary >=2000 and salary < 5000 then "low" 
when salary >=5000 and salary < 10000 then "mid" 
else "high" end as salary_level 
from employees 
order by 1;
```

**11。选择名字中包含“an”的员工**

```
select (first_name) 
from employees 
where lower(first_name) like '%an%';
```

**12。按照格式(_ _ _)-(_ _ _)-(_ _ _ _ _ _)**选择员工的名字和相应的电话号码

```
select concat(first_name, ' ', last_name) employee, replace(phone_number,'.','-') phone_number 
from employees;
```

**13。找到 1994 年 8 月加入的员工。**

```
select concat(first_name, ' ', last_name) employee, 
hire_date 
from employees 
where year(hire_date) = '1994' 
and month(hire_date) = '08';
```

14。编写一个 SQL 查询来显示该公司中收入高于平均工资的员工

```
select concat(emp.first_name,last_name) name, 
emp.employee_id, 
dept.department_name department, 
dept.department_id, 
emp.salary 
from departments dept 
JOIN employees emp on dept.department_id = emp.department_id 
where emp.salary > (select avg(salary) from employees) 
order by dept.department_id;
```

15。找出每个部门的最高工资。

```
select dept.department_id, 
dept.department_name department, 
max(emp.salary)maximum_salary 
from departments dept 
JOIN employees emp on dept.department_id = emp.department_id 
group by dept.department_name, dept.department_id 
order by dept.department_id ;
```

16。编写一个 SQL 查询来显示收入最低的 5 名员工

```
select first_name, 
last_name, 
employee_id, 
salary 
from employees 
order by salary limit 5;
```

17。找到 80 年代雇佣的员工

```
select employee_id,
concat(first_name,' ' , last_name) employee,
hire_date
from employees
where year(hire_date) between 1980 and 1989;
```

18。以相反的顺序显示雇员的名字和姓名

```
select lower(first_name) name, 
lower(reverse(first_name)) name_in_reverse 
from employees;
```

19。查找当月 15 日之后加入公司的员工

```
select employee_id, 
concat(first_name, ' ' , last_name) employee, 
hire_date 
from employees 
where day(hire_date)> 15;
```

20。显示在不同部门工作的经理和报告员工

```
select concat(mgr.first_name,' ',mgr.last_name) manager, concat(emp.first_name,' ',emp.last_name) employee, 
mgr.department_id mgr_dept, 
emp.department_id emp_dept 
from employees emp 
join employees mgr on emp.manager_id = mgr.employee_id 
where emp.department_id != mgr.department_id 
order by 1;
```

仅使用 SQL，我们就可以从数据中获得很多见解。连接不同的表，按不同的字段对数据进行分组，或者对数据使用一些集合操作或函数，可以讲述隐藏在数据内部的故事。

最后，如果您想在使用 SQL 的同时练习一些 python，这里有一些链接:

[数据科学面试的 Python 编程问题—第一部分](https://oindrilasen.com/2019/06/python-programming-questions-for-data-science-interview/)

[2o Python 中的基本编程问题及解决方案](https://oindrilasen.com/2020/07/2o-easy-programming-problems-and-solutions-in-python/)

20 个行为(非技术)面试问题

由于数据是本世纪的新燃料，让我们继续挖掘和探索。此外，让我们学习并不断练习不同的 SQL 技术，因为 SQL 是数据世界中一个强大的工具。

谢谢大家！

*最初发表于 2021 年 4 月 20 日*[*https://oindrilasen.com*](https://oindrilasen.com/2021/04/sql-interview-practice/)*。*