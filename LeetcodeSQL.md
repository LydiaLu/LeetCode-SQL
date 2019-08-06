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
1083.Sales Analysis II
```sql
select distinct buyer_id
from Sales join Product on Product.product_id = Sales.product_id
where Product.product_name = 'S8' and
buyer_id not in (select buyer_id from Sales join Product 
                  on Product.product_id = Sales.product_id 
                  where Product.product_name = 'iPhone');
```
1084.Sales Analysis III
```sql
select distinct p.product_id, p.product_name
from Product p join Sales s on p.product_id = s.product_id
where s.sale_date >= '2019-01-01' and s.sale_date <= '2019-03-31'
and p.product_id not in (
  select product_id from sales as s 
  where s.sale_date < '2019-01-01' or s.sale_date > '2019-03-31'
);
```
197.Rising Temperature
```sql
select Weather.Id
from Weather join Weather w 
  on datediff(Weather.RecordDate, w.RecordDate) = 1
where Weather.Temperature > w.Temperatrue;
```
1113.Reported Posts
```sql
select extra as report_reason, count(distinct post_id) as report_count
from Actions
where action = 'report' and action_date = '2018-07-04'
group by extra;
```
