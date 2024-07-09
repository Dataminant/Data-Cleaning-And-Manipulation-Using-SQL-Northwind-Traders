
# Data cleaning using SQL (Northwind Traders)  

## Processes:

### Detecting and Removing Duplicated
I started by getting all the variables from Orders table so I can look at the summary of the data gathered. I look for duplicates and errors, or patterns that jump out visually. I see duplicated customerID straight away. The Orders table consist of 830 rows. It has 13 columns (CustomerID, EmployeeID, OrderDate, RequiredDate, ShippedDate, ShipVia, Freight, ShipName,ShipAddress, ShipCity, ShipRegion, ShipPostalCode and ShipCountry).  
  
![data overview](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/75c43f95cd29af0b58987d730a1761216a2cc291/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Data%20Overview.jpg)

Next, I choose the CustomerID variables from the Orders table. To find out how many duplicates are there for each Customer ID, I used the ROW_NUMBER function. SQL window function called ROW_NUMBER gives each row in the result set that the query returns a distinct sequential integer. This can be used to provide every row a unique identifier. In essence,  it is grouping by CustomerID and adding a ROW_NUMBER for each row of a set of customer IDs. Each row has a corresponding number. 

```sql
SELECT customerid, shipname, shipaddress, shippostalcode, shipcountry,
ROW_NUMBER () OVER (order by customerid) AS rownun
FROM the_dataminant_orders ;
```

![using row_number](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/75c43f95cd29af0b58987d730a1761216a2cc291/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Uisng%20the%20ROW_NUMBER%20function%20to%20detect%20how%20many%20duplicates%20there%20are%20for%20each%20Customer%20ID.jpg)

Then, I added the ‘Partition by’ clause to partion by Customer ID. This creates an integer for each row per Customer ID. This allows me to see how many duplicates there are for each Customer ID.

```sql
SELECT customerid, shipname, shipaddress, shippostalcode, shipcountry,
ROW_NUMBER () OVER (partition by customerid order by customerid) AS rownun
FROM the_dataminant_orders ;
```
![added partion](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/75c43f95cd29af0b58987d730a1761216a2cc291/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Added%20the%20%E2%80%98Partition%20by%E2%80%99%20clause%20to%20partion%20by%20Customer%20ID..jpg)

I used Common Table Expression(CTE) to remove the many duplicates for every customer ID because there are many of them. A common table expression is a temporary result set that you can reference within a subsequent select statement, also it make complex queries easier to understand by breaking them into smaller and more readable queries. 

For each customer ID, I selected the first row, effectively removing all of the duplicates for each customer ID.  

```sql
WITH CTE_rownun AS (
SELECT customerid, shipname, shipaddress, shippostalcode, shipcountry,
ROW_NUMBER () OVER (partition by customerid order by customerid) AS rownun
FROM the_dataminant_orders
)
SELECT *
FROM CTE_rownun
WHERE rownun = 1 ;
```
![using CTE](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/75c43f95cd29af0b58987d730a1761216a2cc291/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/CTE%20to%20get%20rid%20of%20the%20duplicates%20for%20each%20customer%20ID.jpg)

### Cleaning the data by using Case Statement  
In order to determine the range and how to go about aggregating the rows that meet specific conditions, I set the minimum and maximum values for the freight variable in the Orders table as Low Charge, Medium Charge and High Charge.

```sql
SELECT MIN(freight), MAX(freight)
FROM the_dataminant_orders ;
 ```

![max min freight](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/6569afc3d8e7e0325a0cfe667097dd243a81a96d/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Selected%20the%20minimum%20and%20maximum%20values%20for%20the%20freight%20variable%20in%20the%20Orders%20table.jpg)

I used a case statement to assign freight values between 0 and 50 as "Low Charge" while trying to make freight charges for each order. To give the new column created a name, I used the alias "Charge."

```sql
SELECT freight,
     CASE WHEN freight < 50 THEN 'low charge'
     END as charge
FROM the_dataminant_orders ;
 ```
![](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/f947440d85dd29409c766ab0015813f5bbfc006a/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Case%20statement%20to%20label%20freight%20values.jpg)

For freight values over 200 and between 50 and 200, I added new conditions. There is now a label for every freight in a new column called "Charge."

```sql
SELECT freight,
     CASE WHEN freight < 50 THEN 'low charge'
          WHEN freight between 50 and 200 THEN 'meduim charge'
	  WHEN freight > 200 THEN 'high charge'
     END AS charge
FROM the_dataminant_orders
```

![higher freight](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/dab5f1c2bf51d0a1f50e2cbb5dbcc3ecb172d86c/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Case%20statement%20for%20higher%20freight%20values.jpg)




