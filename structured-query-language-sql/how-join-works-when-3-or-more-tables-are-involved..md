# How JOIN works when 3 or more tables are involved.

When joining three or more tables, the process is as follows:

1. The optimizer devises a plan, which includes a join order. This could be any combination of the tables involved.
2. The query execution engine applies any predicates (WHERE clause) to the first table that doesn’t involve any of the other tables. It selects out the columns mentioned in the JOIN conditions or the SELECT list or the ORDER BY list.
3. This result set is then joined to the second table. For each row, it joins to the second table, applying any predicates that may apply to the second table. This results in another temporary result set.
4. The final table is then joined in and the ORDER BY is applied.

It’s important to note that only two result sets are ever joined together at once and it builds up. This is why the join order is very important to the plan. The optimizer can join tables in any sequence, which would change performance but not correctness. The most fundamental thing the optimizer does is to select the best order for joins. There are many possible optimizations along the way, such as accessing the data using an index for the ORDER BY instead of generating full result sets. There are also various types of joins that can be performed.



* **Good Exercise to practice a query with more then one join:** [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/)

### References

\[1] [https://stackoverflow.com/questions/1083676/understanding-how-join-works-when-3-or-more-tables-are-involved-sql](https://stackoverflow.com/questions/1083676/understanding-how-join-works-when-3-or-more-tables-are-involved-sql)
