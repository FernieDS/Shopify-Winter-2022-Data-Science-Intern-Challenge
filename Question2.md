### Question 2: 
For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

#### A. How many orders were shipped by Speedy Express in total?
##### 54 orders were shipped by Speedy Express.

```{sql1, echo = TRUE}

SELECT ShipperName, COUNT(DISTINCT OrderID) as NumberOfOrders
FROM Orders
LEFT JOIN Shippers
USING (ShipperID)
WHERE ShipperName = 'Speedy Express';

```

#### B. What is the last name of the employee with the most orders?

##### Peacock with 40 orders.
```{sql2, echo = TRUE}
SELECT LastName, Count(DISTINCT OrderID) as NumberOfOrders
FROM Orders
LEFT JOIN Employees
USING (EmployeeID)
GROUP BY LastName
ORDER BY NumberOfOrders DESC
LIMIT 1;
```

#### C. What product was ordered the most by customers in Germany?
##### Boston Crab Meat was ordered the most in Germany with a total order quantity of 160.
```{sql3, eval = FALSE, echo = TRUE}
SELECT ProductName, SUM(Quantity) AS TotalOrders 
FROM Products p, Orders o, OrderDetails od, customers c
WHERE c.CustomerID = o.CustomerID AND c.Country = 'Germany' AND o.OrderID = od.OrderID AND od.ProductID = p.ProductID
GROUP BY ProductName
ORDER BY TotalOrders DESC
LIMIT 1;
```
