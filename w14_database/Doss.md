# 14.2 Open Requests

Write a SQL query to get a list of all buildings and the number of open requests (Requests in which status equals'Open').

# Database Tables

|Apartmenrts||
|------|---|
|AptID|int|
|UnitNumber|varchar(10)|
|BuildingID|int|

|Buildings||
|------|---|
|BuildingID|int|
|ComplexID|int|
|BuildingName|varchar(100)|
|Address|varchar(500)|

|Complexes||
|---------|---|
|ComplexID|int|
|ComplexName|varchar(100)|

|Requests||
|------|---|
|RequestID|int|
|Status|varchar(100)|
|AptID|int|
|Description|varchar(500)|

## Solution

```sql
SELECT BuildingName, ISNULL(Count, 0) as 'Count'
-- Read building Name from Buildings table, but lazy evaluation on Count
FROM Buildings

LEFT JOIN
-- Try join operation Buildings table and ReqCounts table

(SELECT Apartments.BuildingID, count(*) as 'Count'
-- Read Apartments's Building ID from Apartments table
-- count Apartments's Building

 FROM Requests INNER JOIN Apartments
-- Joint Apartments table and Requests table
-- with same AptID

 ON Requests.AptID = Apartments.AptID
 WHERE Requests.Status = 'Open'
-- where Requests.Status = 'Open'

 GROUP BY Apartments.BuildingID) ReqCounts
 -- Group By BuildingID to count
 -- Name table as ReqCounts

 ON ReqCounts.BuildingID = Buildings.BuildingID
 -- condition for joinning!
```