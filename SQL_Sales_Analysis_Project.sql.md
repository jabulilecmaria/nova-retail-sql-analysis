Part 1: Basic SQL Queries 



1.1 

select ProductID, ProductName, UnitPrice

from products 

where category = 'Electronics'

order by 'UnitPrice' DESC; 



1.2

select Region, count(CustomerID) as Number\_of\_Customers

from customers

Group by Region;



1.3

select

&#x20;OrderID,

&#x20;OrderDate, 

&#x20;TotalSales

from sales

order by OrderDate Desc

limit 10;



1.4

select ProductName, Category, UnitPrice

from products 

where UnitPrice < 1000 



1.5 

select Satisfaction, count(FeedbackID) As Feedback\_Count

from customer\_feedback 

Group by 1

Order By Feedback\_Count Desc; 



Part 2: Intermediate SQL Queries 

2.1

Select p.Category, Sum(s.Totalsales) As TotalRevenue, Sum(s.Profit) As TotalProfit

from sales s 

Inner Join products p

On s.ProductID = p.ProductID

group by p.category

order by totalrevenue Desc;



2.2

Select c.CustomerID, CONCAT(c.FirstName, '', c.LastName) As CustomerName, Sum(s.TotalSales) As TotalSpent

From Customers c

Inner Join Sales s

On c.CustomerID = s.CustomerID

Group By c.CustomerID, c.FirstName, c.LastName

Order By TotalSpent Desc

Limit 5; 



2.3 

select Year(OrderDate) As Year, Month(OrderDate) As Month, Sum(TotalSales) As TotalSales

from sales

where Year(OrderDate) = 2024

Group By Year(OrderDate), Month(OrderDate)

Order By Year Asc, Month Asc;



2.4

Select Channel, Count(OrderID) As NumberofOrders, Avg(TotalSales) As AverageOrderValue, Sum(TotalSales) As TotalRevenue

from sales

Group by Channel

Order By TotalRevenue Desc; 



2.5 

Select p.Category, Round(Avg(cf.Rating),2) As AverageRating, Count(cf.FeedbackID) As NumberofReviews

From customer\_feedback cf

Inner Join sales s 

on cf.OrderID = s.OrderID

Inner Join products p

on s.ProductID = p.ProductID

group by p.Category

Having Count(cf.FeedbackID) >= 50

Order By AverageRating Desc; 



Part 3: Advanced SQL Queries



WITH ProductSales AS (

&#x20;   SELECT

&#x20;       p.Category,

&#x20;       p.ProductName,

&#x20;       SUM(s.TotalSales) AS TotalRevenue

&#x20;   FROM Sales s

&#x20;   INNER JOIN Products p

&#x20;       ON s.ProductID = p.ProductID

&#x20;   GROUP BY

&#x20;       p.Category,

&#x20;       p.ProductName

),



RankedProducts AS (

&#x20;   SELECT

&#x20;       Category,

&#x20;       ProductName,

&#x20;       TotalRevenue,

&#x20;       RANK() OVER (

&#x20;           PARTITION BY Category

&#x20;           ORDER BY TotalRevenue DESC

&#x20;       ) AS SalesRank

&#x20;   FROM ProductSales

)



SELECT

&#x20;   Category,

&#x20;   ProductName,

&#x20;   TotalRevenue

FROM RankedProducts

WHERE SalesRank = 1

ORDER BY TotalRevenue DESC; 



3.2 

WITH CustomerAnalysis AS (

&#x20;   SELECT

&#x20;       c.CustomerID,

&#x20;       CONCAT(c.FirstName, ' ', c.LastName) AS CustomerName,

&#x20;       c.Region,

&#x20;       c.Channel AS PrimaryChannel,

&#x20;       SUM(s.TotalSales) AS TotalPurchases,

&#x20;       COUNT(s.OrderID) AS NumberOfOrders,

&#x20;       AVG(s.TotalSales) AS AverageOrderValue

&#x20;   FROM Customers c

&#x20;   INNER JOIN Sales s

&#x20;       ON c.CustomerID = s.CustomerID

&#x20;   GROUP BY

&#x20;       c.CustomerID,

&#x20;       c.FirstName,

&#x20;       c.LastName,

&#x20;       c.Region,

&#x20;       c.Channel

)



SELECT

&#x20;   CustomerID,

&#x20;   CustomerName,

&#x20;   Region,

&#x20;   PrimaryChannel,

&#x20;   TotalPurchases,

&#x20;   NumberOfOrders,

&#x20;   ROUND(AverageOrderValue, 2) AS AverageOrderValue

FROM CustomerAnalysis

WHERE NumberOfOrders > 3

ORDER BY TotalPurchases DESC; 



3.3 

SELECT

&#x20;   p.ProductName,

&#x20;   p.Category,

&#x20;   ROUND(SUM(s.TotalSales), 2) AS TotalSales,

&#x20;   ROUND(SUM(s.Profit), 2) AS TotalProfit,

&#x20;   ROUND(

&#x20;       (SUM(s.Profit) / SUM(s.TotalSales)) \* 100,

&#x20;       2

&#x20;   ) AS ProfitMarginPercentage

FROM Sales s

INNER JOIN Products p

&#x20;   ON s.ProductID = p.ProductID

GROUP BY

&#x20;   p.ProductID,

&#x20;   p.ProductName,

&#x20;   p.Category

ORDER BY ProfitMarginPercentage DESC;



3.4 

WITH YearlySales AS (

&#x20;   SELECT

&#x20;       YEAR(OrderDate) AS SalesYear,

&#x20;       SUM(TotalSales) AS TotalSales

&#x20;   FROM Sales

&#x20;   WHERE YEAR(OrderDate) IN (2023, 2024)

&#x20;   GROUP BY YEAR(OrderDate)

)



SELECT

&#x20;   SalesYear,

&#x20;   ROUND(TotalSales, 2) AS TotalSales,

&#x20;   ROUND(

&#x20;       (

&#x20;           (TotalSales - LAG(TotalSales) OVER (ORDER BY SalesYear))

&#x20;           / LAG(TotalSales) OVER (ORDER BY SalesYear)

&#x20;       ) \* 100,

&#x20;       2

&#x20;   ) AS GrowthPercentage

FROM YearlySales

ORDER BY SalesYear; 



3.5 

WITH RegionalSales AS (

&#x20;   SELECT

&#x20;       c.Region,

&#x20;       SUM(s.TotalSales) AS TotalSales,

&#x20;       COUNT(s.OrderID) AS TotalOrders

&#x20;   FROM Sales s

&#x20;   INNER JOIN Customers c

&#x20;       ON s.CustomerID = c.CustomerID

&#x20;   GROUP BY c.Region

)



SELECT

&#x20;   Region,

&#x20;   ROUND(TotalSales, 2) AS TotalSales,

&#x20;   TotalOrders,

&#x20;   RANK() OVER (ORDER BY TotalSales DESC) AS SalesRank

FROM RegionalSales

ORDER BY SalesRank; 



Part 4: Business Intelligence



4.1

WITH CustomerOrders AS (

&#x20;   SELECT

&#x20;       cf.CustomerID,

&#x20;       CASE

&#x20;           WHEN AVG(cf.Rating) >= 4 THEN 'Highly Satisfied (4-5)'

&#x20;           ELSE 'Less Satisfied (1-3)'

&#x20;       END AS SatisfactionGroup,

&#x20;       COUNT(DISTINCT s.OrderID) AS NumberOfOrders

&#x20;   FROM customer\_feedback cf

&#x20;   INNER JOIN Sales s

&#x20;       ON cf.CustomerID = s.CustomerID

&#x20;   GROUP BY cf.CustomerID

)



SELECT

&#x20;   SatisfactionGroup,

&#x20;   ROUND(AVG(NumberOfOrders), 2) AS AverageOrdersPerCustomer,

&#x20;   COUNT(CustomerID) AS TotalCustomers

FROM CustomerOrders

GROUP BY SatisfactionGroup

ORDER BY AverageOrdersPerCustomer DESC; 



4.2 

WITH DiscountAnalysis AS (

&#x20;   SELECT

&#x20;       CASE

&#x20;           WHEN DiscountPercent = 0 THEN '0%'

&#x20;           WHEN DiscountPercent BETWEEN 1 AND 10 THEN '1-10%'

&#x20;           WHEN DiscountPercent BETWEEN 11 AND 20 THEN '11-20%'

&#x20;           WHEN DiscountPercent BETWEEN 21 AND 30 THEN '21-30%'

&#x20;       END AS DiscountBand,

&#x20;       

&#x20;       TotalSales,

&#x20;       Profit

&#x20;   FROM Sales

)



SELECT

&#x20;   DiscountBand,

&#x20;   ROUND(SUM(TotalSales), 2) AS TotalSales,

&#x20;   ROUND(SUM(Profit), 2) AS TotalProfit,

&#x20;   ROUND(

&#x20;       (SUM(Profit) / NULLIF(SUM(TotalSales), 0)) \* 100,

&#x20;       2

&#x20;   ) AS ProfitMargin

FROM DiscountAnalysis

WHERE DiscountBand IS NOT NULL

GROUP BY DiscountBand

ORDER BY

&#x20;   CASE DiscountBand

&#x20;       WHEN '0%' THEN 1

&#x20;       WHEN '1-10%' THEN 2

&#x20;       WHEN '11-20%' THEN 3

&#x20;       WHEN '21-30%' THEN 4

&#x20;   END;

4.3 

SELECT

&#x20;   p.ProductID,

&#x20;   p.ProductName,

&#x20;   p.Category,

&#x20;   SUM(s.Quantity) AS TotalQuantitySold,

&#x20;   ROUND(SUM(s.TotalSales), 2) AS TotalRevenue,

&#x20;   ROUND(SUM(s.Profit), 2) AS TotalProfit,

&#x20;   ROUND(

&#x20;       (SUM(s.Profit) / NULLIF(SUM(s.TotalSales), 0)) \* 100,

&#x20;       2

&#x20;   ) AS ProfitMarginPercentage

FROM Products p

INNER JOIN Sales s

&#x20;   ON p.ProductID = s.ProductID

GROUP BY

&#x20;   p.ProductID,

&#x20;   p.ProductName,

&#x20;   p.Category

ORDER BY

&#x20;   TotalProfit ASC;

&#x20;



**Part 5: Management Report**



5.1

SELECT

&#x20;   ROUND(SUM(TotalSales), 2) AS TotalRevenue,

&#x20;   ROUND(SUM(Profit), 2) AS TotalProfit,

&#x20;   ROUND(

&#x20;       (SUM(Profit) / NULLIF(SUM(TotalSales), 0)) \* 100,

&#x20;       2

&#x20;   ) AS OverallProfitMargin,

&#x20;   COUNT(DISTINCT OrderID) AS TotalOrders,

&#x20;   COUNT(DISTINCT CustomerID) AS TotalCustomers,

&#x20;   ROUND(AVG(TotalSales), 2) AS AverageOrderValue

FROM Sales

WHERE YEAR(OrderDate) IN (2023, 2024); 



5.2 Insight #1

WITH CustomerOrders AS (

&#x20;   SELECT

&#x20;       cf.CustomerID,

&#x20;       CASE

&#x20;           WHEN AVG(cf.Rating) >= 4 THEN 'Highly Satisfied (4-5)'

&#x20;           ELSE 'Less Satisfied (1-3)'

&#x20;       END AS SatisfactionGroup,

&#x20;       COUNT(DISTINCT s.OrderID) AS NumberOfOrders

&#x20;   FROM customer\_feedback cf

&#x20;   INNER JOIN Sales s

&#x20;       ON cf.CustomerID = s.CustomerID

&#x20;   GROUP BY cf.CustomerID

)



SELECT

&#x20;   SatisfactionGroup,

&#x20;   ROUND(AVG(NumberOfOrders), 2) AS AverageOrdersPerCustomer,

&#x20;   COUNT(CustomerID) AS TotalCustomers

FROM CustomerOrders

GROUP BY SatisfactionGroup

ORDER BY AverageOrdersPerCustomer DESC; 



Insight#2 

SELECT

&#x20;   p.ProductID,

&#x20;   p.ProductName,

&#x20;   p.Category,

&#x20;   SUM(s.Quantity) AS TotalQuantitySold,

&#x20;   ROUND(SUM(s.TotalSales), 2) AS TotalRevenue,

&#x20;   ROUND(SUM(s.Profit), 2) AS TotalProfit,

&#x20;   ROUND(

&#x20;       (SUM(s.Profit) / NULLIF(SUM(s.TotalSales), 0)) \* 100,

&#x20;       2

&#x20;   ) AS ProfitMarginPercentage

FROM Products p

INNER JOIN Sales s

&#x20;   ON p.ProductID = s.ProductID

GROUP BY

&#x20;   p.ProductID,

&#x20;   p.ProductName,

&#x20;   p.Category

ORDER BY TotalProfit ASC;





Insight#3

WITH DiscountAnalysis AS (

&#x20;   SELECT

&#x20;       CASE

&#x20;           WHEN DiscountPercent = 0 THEN '0%'

&#x20;           WHEN DiscountPercent BETWEEN 1 AND 10 THEN '1-10%'

&#x20;           WHEN DiscountPercent BETWEEN 11 AND 20 THEN '11-20%'

&#x20;           WHEN DiscountPercent BETWEEN 21 AND 30 THEN '21-30%'

&#x20;       END AS DiscountBand,

&#x20;       

&#x20;       TotalSales,

&#x20;       Profit

&#x20;   FROM Sales

)



SELECT

&#x20;   DiscountBand,

&#x20;   ROUND(SUM(TotalSales), 2) AS TotalSales,

&#x20;   ROUND(SUM(Profit), 2) AS TotalProfit,

&#x20;   ROUND(

&#x20;       (SUM(Profit) / NULLIF(SUM(TotalSales), 0)) \* 100,

&#x20;       2

&#x20;   ) AS ProfitMargin

FROM DiscountAnalysis

WHERE DiscountBand IS NOT NULL

GROUP BY DiscountBand

ORDER BY

&#x20;   CASE DiscountBand

&#x20;       WHEN '0%' THEN 1

&#x20;       WHEN '1-10%' THEN 2

&#x20;       WHEN '11-20%' THEN 3

&#x20;       WHEN '21-30%' THEN 4

&#x20;   END;







