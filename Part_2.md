
# Instructions for Setting Up the Database for the Flask Kiosk Application

### Step 1: Start XAMPP
1. Open **XAMPP** on your computer.
2. Start the **Apache** and **MySQL** modules by clicking the "Start" buttons next to them.

### Step 2: Open phpMyAdmin
1. Open your web browser and navigate to `http://localhost/phpmyadmin`.
2. Once phpMyAdmin loads, follow the next steps.

### Step 3: Create the Database and Tables
1. Click on the **SQL** tab in phpMyAdmin.
2. Copy and paste the SQL script below into the SQL query box.

```sql
-- Create the database
CREATE DATABASE IF NOT EXISTS db_kiosk;

-- Use the database
USE db_kiosk;

-- Create the `product` table
CREATE TABLE `product` (
  `ProductID` int(11) NOT NULL,
  `ProductName` varchar(255) DEFAULT NULL,
  `Price` decimal(10,2) DEFAULT NULL,
  `Unit` varchar(50) DEFAULT NULL,
  `PhotoURL` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Insert sample data into the `product` table
INSERT INTO `product` (`ProductID`, `ProductName`, `Price`, `Unit`, `PhotoURL`) VALUES
(1, 'Fresh Apples', 2.50, 'kg', 'images/apple.jpg'),
(2, 'Organic Tomatoes', 3.00, 'kg', 'images/orange.jpg'),
(3, 'Green Lettuce', 1.20, 'piece', 'images/lettuce.jpg'),
(4, 'Whole Wheat Bread', 2.00, 'pack', 'images/bread.jpg'),
(5, 'Free Range Eggs', 3.50, 'dozen', 'images/eggs.jpg');

-- Set the primary key for the `product` table
ALTER TABLE `product`
ADD PRIMARY KEY (`ProductID`);

-- Modify `ProductID` to auto-increment
ALTER TABLE `product`
  MODIFY `ProductID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

-- Create the `order` table
CREATE TABLE `order` (
  `OrderID` int(11) NOT NULL,
  `CustomerMobileNumber` varchar(15) DEFAULT NULL,
  `TotalCost` decimal(10,2) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Set the primary key for the `order` table
ALTER TABLE `order`
  ADD PRIMARY KEY (`OrderID`);

-- Modify `OrderID` to auto-increment
ALTER TABLE `order`
  MODIFY `OrderID` int(11) NOT NULL AUTO_INCREMENT;

-- Create the `orderdetails` table
CREATE TABLE `orderdetails` (
  `OrderDetailsID` int(11) NOT NULL,
  `OrderID` int(11) NOT NULL,
  `ProductID` int(11) NOT NULL,
  `Quantity` decimal(10,2) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- Set the primary key and foreign keys for the `orderdetails` table
ALTER TABLE `orderdetails`
  ADD PRIMARY KEY (`OrderDetailsID`),
  ADD KEY `OrderID` (`OrderID`),
  ADD KEY `ProductID` (`ProductID`);

-- Modify `OrderDetailsID` to auto-increment
ALTER TABLE `orderdetails`
  MODIFY `OrderDetailsID` int(11) NOT NULL AUTO_INCREMENT;

-- Add foreign key constraints
ALTER TABLE `orderdetails`
  ADD CONSTRAINT `fk_order` FOREIGN KEY (`OrderID`) REFERENCES `order` (`OrderID`),
  ADD CONSTRAINT `fk_product` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);
```

### Step 4: Execute the SQL Script
1. Click the "Go" button to execute the SQL script.
2. Once executed, you will have a database named **`db_kiosk`** with three tables:
   - **`product`**: Contains product details.
   - **`order`**: Tracks customer orders.
   - **`orderdetails`**: Stores the details of products in each order.

### Step 5: Verify the Setup
1. In phpMyAdmin, navigate to the **db_kiosk** database.
2. Check the **product**, **order**, and **orderdetails** tables to ensure they have been created successfully with the appropriate relationships and sample data.
