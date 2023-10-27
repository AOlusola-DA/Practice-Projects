# Practice-Projects
Data Analysis Using SQL

Create a column to concatenate the employees first and last name.
Add fullname column (placing it after first name column) to employees table

ALTER TABLE employees ADD COLUMN FullName VARCHAR(50) NOT NULL After FirstName;

Edit records in the employees table

UPDATE employees 
SET FullName = concat(FirstName,' ',LastName); 

USE classicmodels database

USE classicmodels;

ADD ORDERAMOUNT COLUMN TO ORDERDETAILS TABLE

ALTER TABLE orderdetails ADD COLUMN OrderAmount DECIMAL(10,2) NOT NULL AFTER priceEach; 

EDIT  RECORDS IN THE  ORDERDETAILS TABLE

UPDATE orderdetails
SET OrderAmount = (quantityOrdered*priceEach);

CREATE A  TEMPORARY TABLE TO CALCULATE THE ORDER AMOUNT BY STATUS

CREATE VIEW `Order Amount By Status` AS
SELECT SUM(od.OrderAmount) OrderAmountSum, o.Status OrderStatus 
FROM orderdetails od 
JOIN orders o ON od.OrderNumber = o.OrderNumber
GROUP BY OrderStatus;

CREATE A TEMPORARY TABLE TO CALCULATE THE TOP 3 CUSTOMERS

CREATE VIEW `Top 3 Customers` AS
SELECT c.FullName C_Name, SUM(p.amount) Purchase
FROM customers c 
JOIN payments p ON c.customerNumber = p.customerNumber
GROUP BY c.FullName
ORDER BY Purchase DESC 
LIMIT 3;

PERFORM A UNIVARIATE ANALYSIS ON THE ORDER STATUS BY CREATING A VIEW TO TRACK THE 'SHIPPED AND CANCELLED' ORDER

CREATE VIEW `Order status count` AS
SELECT status Order_Status, COUNT(Status) Count_Order_Status
FROM orders
GROUP BY status 
HAVING status IN ('shipped', 'cancelled');

CREATE A FUNCTION THAT TAKES IN THE CUSTOMER'S ID AS A PARAMETER, AND OUTPUTS THE AMOUNT OF ORDERS MADE BY THAT CUSTOMER

DELIMITER $$
CREATE PROCEDURE `Get Customer Order Amount`(IN c_id INT) 
BEGIN
SELECT c.customerNumber Customer_ID, od.OrderAmount Amount 
FROM customers c 
JOIN orders o ON c.customerNumber = o.customerNumber
JOIN orderdetails od ON o.orderNumber = od.orderNumber
GROUP BY Customer_ID
HAVING Customer_ID = c_id;
END $$
DELIMITER ; 

Get the Order Amount for Customer ID 202

CALL `Get Customer Order Amount`(202);    


