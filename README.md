# Winter 2022 Data Science Intern Challenge 
## By Ramil Chaimongkolbutr

![Shopifylogo](./images/shopify_logo.jpg)

Please complete the following questions, and provide your thought process/work. You can attach your work in a text file, link, etc. on the application page. Please ensure answers are easily visible for reviewers!

***

**Question 1**: Given some sample data, write a program to answer the following: click here to access the required data set
On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of $3145.13. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis.  

a. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.  
b. What metric would you report for this dataset?  
c. What is its value?

**Question 2**: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

a. How many orders were shipped by Speedy Express in total?  
b. What is the last name of the employee with the most orders?  
c. What product was ordered the most by customers in Germany?  

***

**Note:** For the technical Notebook, please click this [link](./Code_Challenge_Shopify_Ramil C.ipynb)

## Question 1
### a. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data.
From `describe()` function, we can see cleary from where the naive AOV of $3145.13 comes. Without taking a look at the distribution of the data, The arithmetic mean can lead to wrongly represent the central tendency of the data. For example, the extreme values (or outliers) can significantly increase or decrease the mean when they are included in the calculation. As a result, this can make the mean a poor estimate for the AOV.

Let's see the distribution of the boxplots.
![boxplots](./images/boxplots.png)

From the box plots, we can see that there are many extreme order amounts in the column in comparison to one without the outliers. Since all the outliers are extremely further away from any of the other values, it surely can shift our AOV upward, which seems to be the case of ours. **Since the mean is highly affected by the extreme values, we think that there are 2 better ways to evaluate the data:**  
**1. We can use the median**.  
**2. In case we would like to stick with the mean, we can use it by ignoring outliers**.  

### b. What metric would you report for this dataset?
As mentioned above, **the median** of the column would be a better metric to report this dataset since it is not affected by the extreme of some values. 

### c. What is its value?
Therefore, the better way to represent the AOV of the `order_amount` column is to use the **median** of **284.0**. 

***

## Question 2
### a. How many orders were shipped by Speedy Express in total?
```
SELECT COUNT(*) as NumberOrder
FROM Orders JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
WHERE Shippers.ShipperName = "Speedy Express";
```
We join 2 tables between Orders and Shippers to get shipper's name by ShipperID. Then we count the rows whose ShipperName is 'Speedy Express.' The total orders that were shipped by Speedy Express are **54**.

### b. What is the last name of the employee with the most orders?
```
SELECT Employees.LastName, COUNT(*) AS NumberOrder 
FROM Orders JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
GROUP BY Employees.LastName 
ORDER BY NumberOrder DESC
LIMIT 1;
```
We join 2 tables between Orders and Employees to get employees' lastnames and count each of them. Then we order it descendingly and limit the result to one to get the most orders of **40** by the last name, **Peacock**.

### c. What product was ordered the most by customers in Germany?
```
SELECT Products.ProductName, SUM(OrderDetails.Quantity) AS TotalQuantity
FROM Orders JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderId
JOIN Customers ON Orders.CustomerID = Customers.CustomerID
JOIN Products ON OrderDetails.ProductID = Products.ProductID
WHERE Customers.Country = "Germany"
GROUP BY OrderDetails.ProductID
ORDER BY TotalQuantity DESC
LIMIT 1;
```
We have to join 4 tables in this case among Orders, OrderDetail, Customers, and Products. First, we join Orders with OrderDetails to get the ProductID and Quantity. Then we join Orders with Customers to get countries of customers. Lastly we join with Products to get the name of each product. After joining them, we group the data by its ProductID and find the total quantity of each ProductID. We order the total quantity and limit it to one to get **Boston Crab Meat** as the most ordered product by customers in Germany with the total quantity of **160**.

