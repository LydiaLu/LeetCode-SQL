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
