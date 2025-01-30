# E-Commerce-Database

E-COMMERCE DATABASE

This is a database created using MySQL Workbench. It keeps records of available products, prices and customer orders.It is a database for a Fruit&Veggie shop to help keep track of products in stock, monitor sales and targets. It’s purpose it to help with re-stocking and marketing planning. 

Created 4 tables products, categories, orders, customers. Linked them with foreign keys products with categories, categories with products, customers with orders, orders with products and categories.


CREATE TABLE Products (
productID INT PRIMARY KEY,
name VARCHAR(255) NOT NULL, 
Quantity INT NOT NULL,
Price INT,
categoryID INT,
FOREIGN KEY (categoryID) REFERENCES Categories(CategoryID)
);

CREATE TABLE categories (
    categoryID INT,
    name VARCHAR(100) NOT NULL,
    productID INT,
    PRIMARY KEY (categoryID),
    FOREIGN KEY (productID) REFERENCES products(productID) 
);

CREATE TABLE customers (
customerID INT,
name varchar(100) not null,
email varchar(100) not null unique,
orderID int,
primary key (customerID),
FOREIGN KEY (orderID) REFERENCES orders(orderID) 
);

CREATE TABLE orders (
orderID INT,
product varchar(100) not null,
quantity INT not null,
orderDate date not null,
primary key (orderID),
productID INT,
FOREIGN KEY (productID) REFERENCES products(productID),
CategoryID, 
FOREIGN KEY (categoryID) REFERENCES categories(categoryD),
);


Populated tables with values below:


INSERT INTO customers (customerID, name, email)
values (1, "John", "john@mail.com");
values (2, "Sam", "sam@mail.com");
values (3, "Anna", "anna@mail.com");
values (4, "James", "james@mail.com");
values (5, "Jessica", "jessica@mail.com");

INSERT INTO PRODUCTS (productID, name, price, Quantity, categoryID)
values (1, "apples", 1, 100, 1);
values (2, "banana", 0.5, 100, 1);
values (3, "kiwi", 0.6, 100, 1);
values (5, "carrots", 0.6, 100, 2);
values (6, "potato", 0.3, 100, 2);
values (4, "tomato", 0.4, 100, 2);
values (7, "milk", 1.5, 50, 3);
values (8, "cheese", 2.5, 50, 3);
values (9, "cream", 1, 60, 3);
values (10, "pear", 1, 70, 1);

INSERT INTO categories (categoryID, name)
values (1, "fruit");
values (2, "veggie");
values (3, "dairy");

INSERT INTO ORDERS (orderID, product, quantity, productID, orderDate)
values (1, "apples", 10, 1, 121024);
values (2, "milk", 1, 1, 1, 121024);
values (3, "tomatos", 20, 4, 2, 101024);
values (4, "cheese", 3, 8, 2, 101024);
values (5, "pear", 15, 10, 2, 101024);
values (6, "pear", 3, 10, 3, 1051024);
values (7, "carrots", 20, 5, 4, 17122024);
values (8, "apples", 10, 1, 4, 17122024);
values (9, "apples", 10, 1, 5, 20122024);
values (10, "kiwi", 15, 3, 5, 20122024);




QUERIES FOR ANALISIS

-	This code displays products, prices and quantity available.
SELECT name, quantity, price from products;

-	Identify product with low stock, less than 5

SELECT name, quantity 
FROM products
WHERE quantity < 5;


-	Calculate total revenue generated from sales. This code sums total revenue by multiplying product price by quantity orderes and sums is it up.

SELECT SUM(products.price * orders.quantity) as Revenuefrom products
INNER JOIN orders
ON products.productID = orders.productID; 


-	Top 3 best selling products.

SELECT products.name, orders.product, orders.quantity
FROM products
JOIN orders
ON  products.productID = orders.productID
WHERE orders.quantity > 0
ORDER BY quantity desc
LIMIT 3;

-	Show all orders made by specific customer. This code joins info on customer and orders to return all orders made by John.

SELECT customers.name, orders.product, orders.quantity
FROM customers
JOIN orders
ON customers.customerID = orders.customerID
WHERE customers.name = "John";

-	Calculate total number of product sold in each category. This code uses union to display total number of ph
SELECT categories.name, sum(quantity) as totalQuantity
FROM orders
INNER JOIN categories
ON orders.categoryID = categories.categoryID
WHERE categories.categoryID = 3
UNION
SELECT categories.name, sum(quantity) as totalQuantity
FROM orders
INNER JOIN categories
ON orders.categoryID = categories.categoryID
WHERE categories.categoryID = 2
UNION
SELECT categories.name, sum(quantity) as totalQuantity
FROM orders
INNER JOIN categories
ON orders.categoryID = categories.categoryID
WHERE categories.categoryID = 1;
