# Group By and Aggregate Functions



```sql
SELECT project_id,
       Round(Avg(e.experience_years)::NUMERIC, 2) AS average_years
FROM   employee e
       left join project p
              ON p.employee_id = e.employee_id
GROUP  BY p.project_id
ORDER  BY p.project_id 
```

Given the query above.

How does the SQL interpreter know that I only want the average of employees for a particular project?

What prevents the output from being the average of all employees in all projects?



**Answer:**

The `GROUP BY` clause groups the results by `p.project_id`, which means that the `AVG()` function will calculate the average experience years for each distinct project. So, each row in the result set will correspond to a specific project, and the average experience years will be calculated based only on the employees assigned to that project.

If you didn't include the `GROUP BY p.project_id` clause, then the `AVG()` function would calculate the average experience years for all employees in all projects, as you mentioned. However, by including the `GROUP BY p.project_id`, you're instructing the SQL interpreter to group the results by project, thereby calculating the average only within the scope of each project.
