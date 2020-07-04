# 175.Combine Two table
## Describtion
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId is the primary key column for this table.
```
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId is the primary key column for this table.
```
Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:
```FirstName, LastName, City, State```

## Idea
Due to **regardless if there is an address for each of those people**, choose to use left join of Person and Address table.

## Solution
 ```sql
SELECT P.FirstName, P.LastName, A.City, A.State
FROM Person AS P LEFT JOIN Address AS A ON P.PersonID = A.PersonID;
```
