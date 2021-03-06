# Fall2022TechnicalProject
### Technical Project for Shopify Data Science Intern Fall 2022 Application

Below are my answers to the technical problems given, with additional work being in accordingly labeled folders in this repository. 

## Problem 1

#### On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

### Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 

The thing that is wrong with this calculation is the existence of significant outliers in the data. As the box plots show, there is one store that has a signficiantly higher priced shoe than the other stores, and there is one store that is recieving a much greater number of orders than the other stores. Given the volume of orders for the latter store (store 42) being extremely unrealistic at first glance, it is possible that there may be some error in the entry of the order information for the store. The higher priced store could feasibly be a high fashion store of some sorts, but its shoe price detracts from an accurate representation of the AOV across the stores. 

![Alt text](/images/shoe_price.png?raw=true)
![Alt text](/images/order_size.png?raw=true)

A better way to evaluate the data would be to remove outliers (3 standard deviations from the median) from the data set and calculate the average order value from there. 

### What metric would you report for this dataset?

I would stick with the metric being the mean value of each order, as it should give an accurate representation of the AOV accross the stores after the outliers are removed.

### What is its value?

The value of the mean after the outliers are removed is **$300.00**

## Problem 2

#### For this question you???ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

### How many orders were shipped by Speedy Express in total?

SELECT Count(Orders.OrderID)
FROM Orders, Shippers 
WHERE Orders.ShipperID = Shippers.ShipperID 
AND ShipperName = 'Speedy Express'

##### Answer: 54

### What is the last name of the employee with the most orders?

SELECT Count(Orders.OrderID) as OrderCount, Employees.LastName
FROM Employees, Orders
WHERE Orders.EmployeeID = Employees.EmployeeID
GROUP BY Employees.LastName
ORDER BY OrderCount desc
LIMIT 1

##### Answer: Peacock

### What product was ordered the most by customers in Germany?

SELECT Products.ProductName, SUM(OrderDetails.Quantity) as Quantity
FROM Customers, Products, Orders, OrderDetails
WHERE Customers.CustomerID = Orders.CustomerID
AND Orders.OrderID = OrderDetails.OrderID
AND OrderDetails.ProductID = Products.ProductID
AND Customers.Country = 'Germany'
GROUP BY Products.ProductName
ORDER BY Quantity DESC
LIMIT 1

##### Answer: Boston Crab Meat
