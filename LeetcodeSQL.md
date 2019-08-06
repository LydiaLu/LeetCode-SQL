---
title: "Easy/Medium/Hard"
author: "Zhihong LU"
date: "08/06/2019"
output: html_document
---
1141. User Activity for the Past 30 Days I
```sql
select activity_date as day, count(distinct user_id) as active_users
from Activity
where activity_date between '2019-06-28' and '2019-07-27'
group by activity_date;
```
