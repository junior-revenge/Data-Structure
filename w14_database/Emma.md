# Problem 14.4 Joins
What are the different types of joins? Please explain how they differ and why certain types are better in certain situations.

## Solution
JOIN is used to combine the results of  two tables. To perform a JOIN, each of the tables must have at least one field that will be used to find matching records from the other table. The join type defines which records will go into the result set.

### Inner JOIN
The result set would contain only the data where the criteria match. When we only need the intersection of data excluding unmatched recurds, we use it.

### OUTER JOIN
An OUTER JOIN will always contain the result of INNER JOIN, but it may also contain some records that have no mnatching record in the other table. OUTER JOINs are divided into the following subtypes  
**LEFT OUTER JOIN or simply LEFT JOIN**  
The result will contain all records from the left table. If no matching records were found in the right table, then its fields will contain the NULL values.  
**RIGHT OUTER JOIN or simply OUTER JOIN**  
This type of join is the opposite of LEFT JOIN. It will contain every record from the right table( the missing fields from the left table will be NULL). Note that if we have two tables, A and B, then we can say that the statement A LEFT JOIN B is equivalent to the staement B RIGHT JOIN A.  
**FULL OUTER JOIN**  
This type of join combine the results of the LEFT and RIGHT JOINS. All records from both tables will be included in the result set, regardless of whether or not a matching record exists in the other table. If no matching record was found, then the corresponding result fields wil have a NULL value. 

---
### Cross JOIN
It returns the cartesian product of the two tables ( all possible row combinations). 
### SELF JOIN
A table joins with itself. Commonly used to represent hierarchical or recursive relationshpes.

