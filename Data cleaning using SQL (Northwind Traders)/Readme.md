# Data cleaning using SQL (Northwind Traders)  

## Processes:

### Detecting and Removing Duplicated
I started by getting all the variables from Orders table so I can look at the summary of the data gathered. I look for duplicates and errors, or patterns that jump out visually. I see duplicated customerID straight away. The Orders table consist of 830 rows. It has 13 columns (CustomerID, EmployeeID, OrderDate, RequiredDate, ShippedDate, ShipVia, Freight, ShipName,ShipAddress, ShipCity, ShipRegion, ShipPostalCode and ShipCountry).  

  
![data overview](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/399511958396e5d2534d671965183d57a86657f3/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Data%20Overview.jpg)

Next, I choose the CustomerID variables from the Orders table. To find out how many duplicates are there for each Customer ID, I used the ROW_NUMBER function. SQL window function called ROW_NUMBER gives each row in the result set that the query returns a distinct sequential integer. This can be used to provide every row a unique identifier. In essence,  it is grouping by CustomerID and adding a ROW_NUMBER for each row of a set of customer IDs. Each row has a corresponding number.  

![using row_number](https://github.com/Dataminant/Data-cleaning-using-SQL-Northwind-Traders-/blob/399511958396e5d2534d671965183d57a86657f3/Data%20cleaning%20using%20SQL%20(Northwind%20Traders)/Questions/Uisng%20the%20ROW_NUMBER%20function%20to%20detect%20how%20many%20duplicates%20there%20are%20for%20each%20Customer%20ID.jpg)

