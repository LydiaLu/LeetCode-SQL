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
select s.name
from Salesperson s
where s.sales_id not in
  (select sales_id from orders join company 
  on company.com_id = order.sales_id
  where company.name = 'RED');
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
 select round( 
  ifnull(
  (select count(*) from (select distinct requester_id, accepter_id
  from request_accepted) as A
  /
  (select count(*) from (select distinct sender_id, send_to_id
  from friend_request) as B),0)
  ,2) as accept_rate
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
delete p1 from person p1, person p2
where p1.Email = p2.Email and p1.Id > p2.Id;
```
183. Customers Who Never Order
```sql
select Name as Customers
from Customers left join Orders
on Customers.Id = Orders.CustomerId
where CustomerId is null;
```
586. Customer Placing the Largest Number of Orders
```sql
select customer_number from orders
group by customer_numer
order by count(*) desc
limit 1;
```
603. Consecutive Available Seats
```sql
select distinct c1.seat_id 
from cinema c1 join cinema c2 on abs(c1.seat_id-c2.seat_id) = 1
where c1.free = true and c2.free = true
order by seat_id;
```
175. Combine Two Tables
```sql
select Firstname, Lastname, City, State
from Person left join Address 
on Person.PersonId = Address.PersonId;
```
596. Classes More Than 5 Students
```sql
select class from courses
group by class 
having count(distinct student) >=5;
```
619. Biggest Single Number
```sql
select max(num) as num from my_numbers
where num not in 
  (select num from my_numbers 
  group by num 
  having count(*)>1);
```
595. Big Countries
```sql
select name, population, are
from World
where area > 3000000 or population > 25000000;
```
1050. Actors and Directors Who Cooperated At Least Three Times
```sql
select actor_id, director_id
from ActorDirector
group by actor_id, director_id
having count(*)>=3;
```
574. Winning Candidate
```sql
select Name from Candidate
where id = (
    select CandidateId
    from Vote
    group by CandidateId
    order by count(*) desc
    limit 1);
```
1098. Unpopular Books
```sql
```
608. Tree Node
```sql
select id, 
case when p_id is null then 'Root'
when id in (select distinct p_id from tree) the 'Inner'
else 'Leaf' end as Type
from tree
order by id;
```
612. Shortest Distance in a Plane
```sql
select round(sqrt(min(pow(p1.x-p2.x,2)+pow(p1.y-p2.y,2)))),2) as shortest
from point_2d p1 join point_2d p2
on p1.x <> p2.x or p1.y <> p2.y;
```
614. Second Degree Follower
```sql
```
1132. Reported Posts II
```sql
```
178. Rank Scores
```sql
select Score, dense_rank() over(order by Score desc) as Rank
from Scores;
```
1077. Project Employees III
```sql
select p.project_id, p.employee_id
from (select p.project_id, p.employee_id, 
      dense_rank() over(partition by project_id order by e.experience_years desc) as rank
      from Project p join Employee e 
      on p.employee_id = e.employee_id) as t
where rank = 1;
```
1070. Product Sales Analysis III
```sql
select s.product_id, year as first_year, quantity, price
from 
  (select s.*, rank() over(partition by product_id order by year) as rank 
  from Sales s) as t
where rank = 1;
```
177. Nth Highest Salary
```sql
select distinct Salary as getNthHighestSalary
from Employee
order by Salary desc
limit 1 offset n-1;
```
1107. New Users Daily Count
```sql

```
570. Managers with at least 5 Direct Reports
```sql
select e2.Name 
from Employee e1 join employee e1
on e1.ManagerId = e2.Id
group by e2.Id
having count(*) >=5;
```
585. Investments in 2016
```sql
select sum(TIV_2016) as TIV_2016
from insurance
where insurance.TIV_2015 in 
  (select TIV_2015 from insurance group by TIV_2015
  having count(*) > 1)
and concat(LAT, LON) in 
  (select concat(LAT, LON) from insurance
  group by LAT, LON
  count(*)=1);
```
1112. Highest Grade For Each Student
```sql
select student_id, course_id, grade
from 
  (select *, row_number() over(partition by student_id order by grade desc) as rank
  from Employee) as t1
where rank = 1
order by student_id;
```
578. Get Highest Answer Rate Question
```sql
select question_id as survey_log
from (select question_id, sum(case when action = 'answer' then 1 else 0 end) as num_answer,
      sum(case when action = 'show' then 1 else 0 end) as num_show
      from survey_log
      group by question_id) as tbl
order by (num_answer/number_show) desc
limit 1;
```
534. Game Play Analysis III
```sql
select player_id, device_id, event_date, 
  sum(games_played) over(partition by player_id order by event_date
  rows between unbounded preceding and current row) as games_played_so_far
from Activity
```
550. Game Play Analysis IV
```sql
```
602. Friend Requests II: Who Has the Most Friends
```sql
```
626. Exchange Seats
```sql
select(case when mod(id, 2) <> 0 and id <> counts then id+1
when mod(id,2) <> 0 and id = counts then id
else id -1 end) as id, student
from seat, (select count(distinct id) as counts from seat) as seat_counts
order by id;
```
184. Department Highest Salary
```sql
select Department.Name as Department,
Employee.Name as Employee, Salary
from (select Department.Name, Employee.Name, Salary, rank() over(partition by Department.name
      order by Salary desc) as rank 
      from Department join Employee on Department.Id = Employee.DepartmentId) as t1
where rank = 1;
```
1045. Customers Who Bought All Products
```sql
select customer_id from CUstomer
group by customer_id
having count(distinct product_key) = (select count(*) from Product);
```
580. Count Student Number in Departments
```sql
select dept_name, count(distinct student_id) as student_number
from department left join student
on student.dept_id = department.dept_id
group by dept_name
order by student_number desc, depat_name;
```
180. Consecutive Numbers
```sql
select distinct l1. Num as ConsecutiveNums
from logs l1, logs, l2, log l3
where l1.Id = l2.Id-1 and l2.Id = l3.Id - 1 and l1. Num = l2.Num and l2.Num = l3.Num;
```
1126. Active Businesses
```sql
select business_id
from 
  (select *, avg(occurences) over(partition by event_type) as mean
  from events) as t1
where occurences > mean
group by business_id
having count(business_id)>1;
```
