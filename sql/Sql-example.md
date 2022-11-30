# 1. Multi order by

```sql
-- A list of all calls (sorted by employee and start time)
SELECT *
FROM call
ORDER BY
    call.employee_id ASC,
    call.start_time ASC;
```

# 2. Datediff
> Return the number of days between two date values:

```sql
-- A list of all calls together with the call duration
SELECT 
    call.*,
    DATEDIFF("SECOND", call.start_time, call.end_time) AS call_duration
FROM call
ORDER BY
    call.employee_id ASC,
    call.start_time ASC;

```

# 3. datediff and Aggregate
```sql
-- SUM of call duration per each employee
SELECT 
    employee.id,
    employee.first_name,
    employee.last_name,
    SUM(DATEDIFF("SECOND", call.start_time, call.end_time)) AS call_duration_sum
FROM call
INNER JOIN employee ON call.employee_id = employee.id
GROUP BY
    employee.id,
    employee.first_name,
    employee.last_name
ORDER BY
    employee.id ASC;

```

# 4. Calculating Ratio
```sql
-- % of call duration per each employee compared to the duration of all his calls
SELECT 
    employee.id,
    employee.first_name,
    employee.last_name,
    call.start_time, 
    call.end_time,
    DATEDIFF("SECOND", call.start_time, call.end_time) AS call_duration,
    duration_sum.call_duration_sum,
    CAST( CAST(DATEDIFF("SECOND", call.start_time, call.end_time) AS DECIMAL(7,2)) / CAST(duration_sum.call_duration_sum AS DECIMAL(7,2)) AS DECIMAL(4,4)) AS call_percentage
FROM call
INNER JOIN employee ON call.employee_id = employee.id
INNER JOIN (
    SELECT 
        employee.id,
        SUM(DATEDIFF("SECOND", call.start_time, call.end_time)) AS call_duration_sum
    FROM call
    INNER JOIN employee ON call.employee_id = employee.id
    GROUP BY
        employee.id
) AS duration_sum ON employee.id = duration_sum.id
ORDER BY
    employee.id ASC,
    call.start_time ASC;

```

# 5. Average
```sql
-- average call duration per employee
SELECT 
    employee.id,
    employee.first_name,
    employee.last_name,
    AVG(DATEDIFF("SECOND", call.start_time, call.end_time)) AS call_duration_avg
FROM call
INNER JOIN employee ON call.employee_id = employee.id
GROUP BY
    employee.id,
    employee.first_name,
    employee.last_name
ORDER BY
    employee.id ASC;
 
-- average call duration - all calls
SELECT
    AVG(DATEDIFF("SECOND", call.start_time, call.end_time)) AS call_duration_avg
FROM call;

```

# 6. Compare Avg value
```sql
-- the difference between AVG call duration per employee and AVG call duration
SELECT 
    single_employee.id,
    single_employee.first_name,
    single_employee.last_name,
    single_employee.call_duration_avg,
    single_employee.call_duration_avg - avg_all.call_duration_avg AS avg_difference
FROM
(
    SELECT 
        1 AS join_id,
        employee.id,
        employee.first_name,
        employee.last_name,
        AVG(DATEDIFF("SECOND", call.start_time, call.end_time)) AS call_duration_avg
    FROM call
    INNER JOIN employee ON call.employee_id = employee.id
    GROUP BY
        employee.id,
        employee.first_name,
        employee.last_name
) single_employee
    
INNER JOIN
    
(
    SELECT
        1 AS join_id,
        AVG(DATEDIFF("SECOND", call.start_time, call.end_time)) AS call_duration_avg
    FROM call
) avg_all ON avg_all.join_id = single_employee.join_id;

```
