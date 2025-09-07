# 14.2 Open Requests

Write a SQL query to get a list of all buildings and the number of open requests (Requests in which status equals'Open').

# Database Tables

|Apartmenrt||
|------|---|
|AptID|int|
|UnitNumber|varchar(10)|
|BuildingID|int|

|Apartmenrt||
|------|---|
|BuildingID|int|
|ComplexID|int|
|BuildingName|varchar(100)|
|Address|varchar(500)|

|Requests||
|------|---|
|RequestID|int|
|Status|varchar(100)|
|AptID|int|
|Description|varchar(500)|