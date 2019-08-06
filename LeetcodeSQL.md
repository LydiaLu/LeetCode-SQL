---
title: "Easy/Medium/Hard"
author: "Zhihong LU"
date: "06/08/2019"
output: html_document
---
1141. User Activity for the Past 30 Days I
```sql
select activity_date as day, count(distinct user_id) as active_users
from Activity
where activity_date between '2019-06-28' and '2019-07-27'
group by activity_date;
```

1142. User Activity for the Past 30 Days II
```sql
select round(coalesce(avg(a.num),0),2) as average_sessions_per_user
from (select user_id, count(distinct_session_id) as num from Activity
where activity_date between '2019-06-28' and '2019-07-27'
group by user_id) a;
```
610. Triangle Judgement
```sql
select x, y, z, 
case when (x + y > z and x + z > y and y + z > x) then 'Yes' else 'No' end as triangle
from triangle;
```
627. Swap Salary
```sql
update salary set
sex = case when sex = 'm' then 'f'
else 'm' end;
```

613. Shortest Distance in a Line
```sql
select min(abs(p1.x - p2.x)) as shortest
from point p1 join point p2
on p1.x != p2.x;
```
176. Second Highest Salary
```sql
select 
  ifnull((select Salary
          from Employee
          order by Salary desc
          limit 1 offset 1),0) 
as SecondHighestSalary
```

607. Sales Person
```sql

```
1082.Sales Analysis I
```sql
select seller_id
from Sales
group by seller_id
having sum(price) = (select sum(price) as num from Sales group by seller_id
order by num desc limit 1);
```
1083. Sales Analysis II
```sql
select distinct buyer_id
from Sales join Product on Product.product_id = Sales.product_id
where Product.product_name = 'S8' and
buyer_id not in (select buyer_id from Sales join Product 
                  on Product.product_id = Sales.product_id 
                  where Product.product_name = 'iPhone');
```
1084. Sales Analysis III
```sql
select distinct p.product_id, p.product_name
from Product p join Sales s on p.product_id = s.product_id
where s.sale_date >= '2019-01-01' and s.sale_date <= '2019-03-31'
and p.product_id not in (
  select product_id from sales as s 
  where s.sale_date < '2019-01-01' or s.sale_date > '2019-03-31'
);
```
197. Rising Temperature
```sql
select Weather.Id
from Weather join Weather w 
  on datediff(Weather.RecordDate, w.RecordDate) = 1
where Weather.Temperature > w.Temperatrue;
```
1113. Reported Posts
```sql
select extra as report_reason, count(distinct post_id) as report_count
from Actions
where action = 'report' and action_date = '2018-07-04'
group by extra;
```
1075. Project Employees I
```sql
select p.project_id, round(avg(e.experience_years), 2) as average_years
from Project p join Employee e 
on p.employee_id = e.employee_id
group by p.project_id;
```
1065. Project Employees II
```sql
select project_id
from Project 
group by project_id
having count(distinct employee_id) = 
       (select count(*)
       from Project group by project_id
       order by count(*) desc
       limit 1);
```
1068. Product Sales Analysis I
```sql
select product_name, year, price
from Sales
left join Product on Product.product_id = Sales.product_id;
```
1069. Product Sales Analysis II
```sql
select product_id, sum(quantity) as total_quantity
from Sales
group by product_id;
```
620. Not Boring Movies
```sql
select * from cinema
where mod(id, 2) = 1 and description <> 'boring'
order by rating desc;
```
511. Game Play Analysis I
```sql
select player_id, min(event_date) as first_login
from Activity
group by player_id;
```
512. Game Play Analysis II
```sql
select player_id, device_id 
from Activity
where (player_id, event_date) in 
  (select player_id, min(event_date) 
  from Activity 
  group by player_id);
 ```
 597. Friend Requests I: Overall Acceptance Rate
 ```sql
 ```
 584. Find Customer Referee
 ```sql
 select name
 from customer
 where referee_id <> 2 or referee_id is null;
 ```
181. Employees Earning More Than Their Managers
```sql
select e1.Name as Employee
from Employee e1 join Employee e2
on e1.ManagerId = e2.id
where e1.Salary > e2.Salary;
```
577. Employee Bonus
```sql
select name, bonus
from Employee left join Bonus 
on Employee.empId = Bonus.empId
where bonus < 1000 or bonus is null;
```
182. Duplicate Emails
```sql
select distinct p1.Email
from Person p1 join Person p2 
on p1.Email = p2.Email
group by p1.Id
having count(*) > 1;
```
196. Delete Duplicate Emails
```sql
```
183. Customers Who Never Order
```sql
select Name as Customers
from Customers left join Orders
on Customers.Id = Orders.CustomerId
where CustomerId is null;
```

