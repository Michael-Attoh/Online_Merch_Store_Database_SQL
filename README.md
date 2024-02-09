# Online_Merch_Store_Database_SQL
SQL scripts for an online merchandise store database created in Azure Data Studio. This repository includes the database schema, sample data, and scripts for common queries. Feel free to use, contribute, or provide feedback. For detailed instructions, refer to the README.md file.


This repository contains SQL scripts for creating and populating a database for an online merchandise store. The database is designed to manage users, products, orders, carts, reviews, discounts, shipping information, and user roles.

## Database Schema

### User Table
- UserID (Primary Key)
- Username (Unique)
- Password
- Email (Unique)
- FirstName
- LastName
- Address
- Phone
- UserType (Check: 'Customer', 'Admin', 'Moderator')

### Product Table
- ProductID (Primary Key)
- Name
- Description
- Price
- StockQuantity
- CategoryID (Foreign Key to Category Table)
- BrandID (Foreign Key to Brand Table)
- CreatedBy (Foreign Key to User Table)

### Category Table
- CategoryID (Primary Key)
- Name

### Brand Table
- BrandID (Primary Key)
- Name
- Description

### Order Table
- OrderID (Primary Key)
- UserID (Foreign Key to User Table)
- OrderDate
- TotalAmount
- ShippingAddress
- PaymentStatus (Check: 'Pending', 'Paid', 'Failed')
- ShippingStatus (Check: 'Processing', 'Shipping', 'Delivered')

### OrderItem Table
- OrderItemID (Primary Key)
- OrderID (Foreign Key to Order Table)
- ProductID (Foreign Key to Product Table)
- Quantity
- Subtotal

### Cart Table
- CartID (Primary Key)
- UserID (Foreign Key to User Table)

### CartItem Table
- CartItemID (Primary Key)
- CartID (Foreign Key to Cart Table)
- ProductID (Foreign Key to Product Table)
- Quantity

### Review Table
- ReviewID (Primary Key)
- ProductID (Foreign Key to Product Table)
- UserID (Foreign Key to User Table)
- Rating
- Comment
- ReviewDate

### Discount Table
- DiscountID (Primary Key)
- Code (Unique)
- Percentage
- ExpiryDate

### ShippingInfo Table
- ShippingInfoID (Primary Key)
- OrderID (Foreign Key to Order Table)
- ShipmentDate
- TrackingNumber

### UserRole Table
- UserRoleID (Primary Key)
- UserID (Foreign Key to User Table)
- Role (Check: 'Customer', 'Admin', 'Moderator')

## Sample Data Insertion

Sample data for users, brands, categories, products, orders, order items, carts, cart items, reviews, discounts, shipping information, and user roles has been inserted for demonstration purposes.

## Getting Started

1. **Create Database:**
   ```sql
   CREATE DATABASE OnlineMerchStore
   USE OnlineMerchStore
CREATE TABLE [User](            --- [] makes 'User' an Identifier, because User is a reserved keyword
    UserID INT IDENTITY PRIMARY KEY,
    Username VARCHAR(30) UNIQUE,
    Password VARCHAR(50),
    Email VARCHAR(50) UNIQUE,
    FirstName VARCHAR(30),
    LastName VARCHAR(30),
    Address VARCHAR(50),
    Phone VARCHAR(14),
    UserType VARCHAR(30) CHECK (UserType IN ('Customer', 'Admin', 'Moderator'))
);
select * from [User]

CREATE TABLE Product(
    ProductID INT IDENTITY PRIMARY KEY,
    Name VARCHAR(50),
    Description TEXT,
    Price DECIMAL(8,2),
    StockQuantity INT,
    CategoryID INT REFERENCES Category(CategoryID),
    BrandID INT REFERENCES Brand(BrandID),
    CreatedBy INT REFERENCES [User](UserID) 
);

CREATE TABLE Category(
    CategoryID INT IDENTITY PRIMARY KEY,
    Name VARCHAR(50)
);

CREATE TABLE Brand(
    BrandID INT IDENTITY PRIMARY KEY,
    Name VARCHAR(50),
    Description TEXT
);

CREATE TABLE [Order](            -----[Order] could have been replaced by OrderTable or something else.
    OrderID INT IDENTITY PRIMARY KEY,
    UserID INT REFERENCES [User](UserID),
    OrderDate DATETIME,
    TotalAmount DECIMAL(8,2),
    ShippingAddress VARCHAR(50),
    PaymentStatus VARCHAR(30) CHECK (PaymentStatus IN ('Pending', 'Paid', 'Failed')),
    ShippingStatus VARCHAR(30) CHECK (ShippingStatus IN ('Processing', 'Shipping', 'Delivered'))
);

CREATE TABLE OrderItem(
    OrderItemID INT IDENTITY PRIMARY KEY,
    OrderID INT REFERENCES [Order](OrderID),
    ProductID INT REFERENCES Product(ProductID),
    Quantity INT,
    Subtotal DECIMAL(8,2)
);

CREATE TABLE Cart(
    CartID INT IDENTITY PRIMARY KEY,
    UserID INT REFERENCES [User](UserID)
);

CREATE TABLE CartItem(
    CartItemID INT IDENTITY PRIMARY KEY,
    CartID INT REFERENCES Cart(CartID),
    ProductID INT REFERENCES Product(ProductID),
    Quantity INT
);

CREATE TABLE Review(
    ReviewID INT IDENTITY PRIMARY KEY,
    ProductID INT REFERENCES Product(ProductID),
    UserID INT REFERENCES [User](UserID),
    Rating INT,
    Comment TEXT,
    ReviewDate DATETIME
);

CREATE TABLE Discount(
    DiscountID INT IDENTITY PRIMARY KEY,
    Code VARCHAR(30) UNIQUE,
    Percentage DECIMAL(5,2),
    ExpiryDate DATE
);

CREATE TABLE ShippingInfo(
    ShippingInfoID INT IDENTITY PRIMARY KEY,
    OrderID INT REFERENCES [Order](OrderID),   ---Again, [] makes the reserved keyword an identifier
    ShipmentDate DATETIME,
    TrackingNumber VARCHAR(30)
);

CREATE TABLE UserRole(
    UserRoleID INT IDENTITY PRIMARY KEY,
    UserID INT REFERENCES [User](UserID),
    Role VARCHAR(30) CHECK (Role IN ('Customer', 'Admin', 'Moderator'))
);
------------

-- INPUT/INSERT DATA
select * from [User]  ---To see the columns so we can Insert accordingly 

INSERT INTO [User](UserName, Password, Email, Firstname, LastName, Address, Phone, UserType)
VALUES  ('TBuy1', 'tonnybuys123', 'tbuy34@gmail.com', 'Tonny', 'Bond', '122 Roseland ave', '214-489-2645', 'Admin'),
('MistyLane1', 'Misty123', 'MistyLane14@gmail.com', 'Misti', 'Lane', '314 malibu Lane', '214-489-2645', 'Moderate'),
('MNax1', 'mnanny123', 'mnax12@gmail.com', 'Manne', 'Norty', '222 Washington St', '215-487-9945', 'Customer'),
('YMate1', 'matenat123', 'ymate13@gmail.com', 'Young', 'Choe', '101 Riverside ave', '217-489-8845', 'Customer'),
('TPain1', 'panpan123', 'tpain909@gmail.com', 'Palice', 'Price', '321 Gustyland ave', '216-489-2633', 'Customer'),
('MMusty1', 'mistikal123', 'mamistkal99@gmail.com', 'Rachel', 'Thomas', '34 Milkland ave', '219-489-2848', 'Customer');


SELECT * FROM Brand

INSERT INTO Brand(Name, Description)
values ('Apple', 'ARM-based system '),('Apple', '60 Hz refresh rate 6.10inch touchscreen'),
('HP', '12th Generation Intel Core i5 processor'),('Dell', 'Intel Core i7-13700H'),
('Dell', '12th Gen Intel® Core™ i7-1250U'),('Apple', 'macOS 12 Monterey');

SELECT * FROM Category

INSERT INTO Category(Name)
VALUES ('Tech'), ('Tech'),('Tech'),('Tech'),('Tech'),('Tech');

SELECT * FROM Product  

INSERT INTO Product (Name, Description, Price, StockQuantity, CategoryID, BrandID, CreatedBy)
VALUES 
    ('MacBook M3', 'ARM-based system', 3799.00, '5', 7, 13, 42),
    ('Iphone 15', '60 Hz refresh rate 6.10inch', 1199.00, '3',8,14,43),
    ('HP Pavilion Laptop 14', '12th Gen. Intel Core i5 processor', 529.00, '2',9,15, 44),
    ('Dell Inspiron 7630', 'Intel Core i7-13700H', 649.00, 2, 10, 16, 45),
    ('Dell XPS 9315 SSD', '12th Gen Intel Core i7-1250U', 799.00, '6',11, 17,46),
    ('MacBook Air M2', 'macOS 12 Monterey', 1899.00, '3',12, 18, 47);

SELECT * FROM [Order]

INSERT INTO [Order](UserID, OrderDate, TotalAmount, ShippingAddress, PaymentStatus, ShippingStatus)
VALUES (42,'2024-01-03', 3799.00, '122 Roseland ave', 'Paid','Shipping'),
        (43, '2024-02-01', 1199.00, '314 malibu Lane', 'Pending','Processing'),
        (44, '2024-02-03', 529.00, '222 Washington St', 'Paid','Delivered'),
        (45, '2024-02-03', 649.00, '101 Riverside ave', 'Paid','Processing'),
        (46,'2024-02-06', 799.00, '321 Gustyland ave', 'Paid','Processing'),
        (47, '2024-02-06', 1899.00, '34 Milkland ave', 'Pending','Shipping');

Select *  From OrderItem

SET IDENTITY_INSERT OrderItem off
INSERT INTO OrderItem(OrderID, ProductID, Quantity, Subtotal)
VALUES ('13','29', '5', 3799),
        ('14','30', '3', 1199),
        ('15','31', '2', 529),
        ('16','32', '2', 649),
        ('17','33', '6', 799),
        ('18','34', '3', 1899);

select *  From Cart

INSERT INTO Cart(UserID)
VALUES (42), (43),(44),(45),(46),(47);


Select * from CartItem
INSERT INTO CartItem(CartID, ProductID, Quantity)
VALUES (1, 29, 5),
        (2, 30, 3),
        (3,31,2),
        (4, 32, 2),
        (5, 33, 6),
        (6, 34, 3);

select * From Review

INSERT INTO Review(ProductID, UserID, Rating, Comment, ReviewDate)
VALUES (29, 42, 9, 'Faster & better than I expected', '2023-05-08 01:20:45'),
        (30, 43, 8, 'Battery not great', '2023-01-09 11:10:25'),
        (31, 44, 7, 'Good product', '2021-08-23 09:30:15'),
        (32, 45, 8, 'Good for programming', '2022-05-04 15:20:05'),
        (33, 46, 8, 'Good for gaming', '2023-09-01 09:20:32'),
        (34, 47, 9, 'Amazing product', '2023-06-04 019:10:03');

select * from Discount
INSERT INTO Discount(Code, Percentage, ExpiryDate)
VALUES (2932,15,'2025-02-25'),
        (3324,10,'2026-10-15'),
        (3243,18,'2024-06-05'),
        (9089,30,'2028-12-15'),
        (7656,25,'2027-07-23'),
        (2354,20,'2028-04-13');

select * from ShippingInfo
INSERT INTO ShippingInfo(OrderID, ShipmentDate, TrackingNumber)
 VALUES (13,'2024-01-10 10:21:32', 352637462233),
        (14,'2024-02-12 09:12:11', 100002738472),
        (15,'2024-02-13 10:01:05', 124221100990),
        (16,'2024-02-15 15:32:05', 456688009811),
        (17,'2024-02-18 17:01:20', 100223889010),
        (18,'2024-02-14 18:23:09',120001276590);

select * from UserRole
INSERT INTO UserRole(UserID, Role)
VALUES (42,'Admin'),
        (43,'Moderator'),
        (44,'Customer'),
        (45,'Customer'),
        (46,'Customer'),
        (47,'Customer');

--DONE. ---  DATABASE now Created 
