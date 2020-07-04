#  177. Nth Highest Salary
## Describtion
Write a SQL query to get the nth highest salary from the Employee table.
 ```
 +----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
 ```
 For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.
  ```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
  ```
## Idea
Using function `DENSE_RANK()` to allocate a ranking value to each salary value, and then find the value whose ranking value is n.
```
DENSE_RANK() OVER ( PARTITION BY <expression>[{,<expression>...}]
            ORDER BY <expression> [ASC|DESC], [{,<expression>...}]) 
```
## Solution
 ```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      SELECT Salary FROM( SELECT DISTINCT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS rankn
                          FROM Employee) a
                          WHERE a.rankn = N
  );
END
 ```
