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
