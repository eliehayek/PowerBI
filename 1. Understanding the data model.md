## Relationship handling

The first difference between SQL and DAX is in the way relationships work in the model. In SQL, we can set foreign keys between 
tables to declare relationships, but the engine never uses these foreign keys in queries unless we are explicit about them. 
For example, if we have a Customers table and a Sales table, where CustomerKey is a primary key in Customers and a foreign key in Sales, 
we can write the following query:

```sql
SELECT
   Customers.CustomerName,
   SUM ( Sales.SalesAmount ) AS SumOfSales
FROM Sales
   INNER JOIN Customers
    ON Sales.CustomerKey = Customers.CustomerKey
GROUP BY
   Customers.CustomerName
   ```
  Though we declare the relationship in the model using foreign keys, we still need to be explicit and state the join condition in the query. 
  Although this approach makes queries more verbose, it is useful because you can use different join conditions in different queries, giving 
  you a lot of freedom in the way you express queries. 
  
  In DAX, relationships are part of the model, and they are all LEFT OUTER JOINs. When they are defined in the model, you no longer need 
  to specify the join type in the query: DAX uses an automatic LEFT OUTER JOIN in the query whenever you use columns related to the primary table. 
  Thus, in DAX you would write the previous SQL query as follows:
```sql  
EVALUATE
SUMMARIZECOLUMNS (
    Customers[CustomerName],
    "SumOfSales", SUM ( Sales[SalesAmount] )
)
   ```
   
Because DAX knows the existing relationship between Sales and Customers, it does the join automatically following the model. Finally, the SUMMARIZECOLUMNS 
function needs to perform a group by Customers[CustomerName], but we do not have a keyword for that: 
SUMMARIZECOLUMNS automatically groups data by selected columns.
