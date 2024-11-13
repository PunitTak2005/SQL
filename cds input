-- Step 1: Create the database
CREATE DATABASE CourierDeliverySystem;
USE CourierDeliverySystem;

-- Step 2: Create tables with constraints
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(50),
    Address VARCHAR(100),
    PhoneNumber VARCHAR(15),
    Email VARCHAR(50) UNIQUE
);

CREATE TABLE Couriers (
    CourierID INT PRIMARY KEY,
    Name VARCHAR(50),
    PhoneNumber VARCHAR(15) UNIQUE,
    VehicleType VARCHAR(30),
    Area VARCHAR(50)
);

CREATE TABLE Packages (
    PackageID INT PRIMARY KEY,
    Description VARCHAR(100),
    Weight DECIMAL(5, 2),
    Price DECIMAL(10, 2),
    CustomerID INT,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

CREATE TABLE Deliveries (
    DeliveryID INT PRIMARY KEY,
    PackageID INT,
    CourierID INT,
    DeliveryDate DATE,
    Status VARCHAR(20),
    FOREIGN KEY (PackageID) REFERENCES Packages(PackageID),
    FOREIGN KEY (CourierID) REFERENCES Couriers(CourierID)
);

-- Step 3: Insert sample data
INSERT INTO Customers VALUES (1, 'Alice', '123 Maple St', '1234567890', 'alice@example.com');
INSERT INTO Customers VALUES (2, 'Bob', '456 Oak St', '0987654321', 'bob@example.com');
INSERT INTO Customers VALUES (3, 'Charlie', '789 Pine St', '1112223333', 'charlie@example.com');
INSERT INTO Customers VALUES (4, 'David', '101 Birch St', '2223334444', 'david@example.com');
INSERT INTO Customers VALUES (5, 'Eva', '202 Cedar St', '3334445555', 'eva@example.com');

INSERT INTO Couriers VALUES (1, 'Courier A', '5551234567', 'Bike', 'Downtown');
INSERT INTO Couriers VALUES (2, 'Courier B', '5559876543', 'Car', 'Uptown');
INSERT INTO Couriers VALUES (3, 'Courier C', '5551112222', 'Truck', 'Suburbs');
INSERT INTO Couriers VALUES (4, 'Courier D', '5553334444', 'Bike', 'City Center');
INSERT INTO Couriers VALUES (5, 'Courier E', '5555556666', 'Car', 'Outskirts');

INSERT INTO Packages VALUES (1, 'Laptop', 2.5, 1200.00, 1);
INSERT INTO Packages VALUES (2, 'Books', 1.2, 30.00, 2);
INSERT INTO Packages VALUES (3, 'Phone', 0.5, 800.00, 3);
INSERT INTO Packages VALUES (4, 'Furniture', 15.0, 500.00, 4);
INSERT INTO Packages VALUES (5, 'Groceries', 5.0, 100.00, 5);

INSERT INTO Deliveries VALUES (1, 1, 1, '2024-11-12', 'Delivered');
INSERT INTO Deliveries VALUES (2, 2, 2, '2024-11-13', 'In Transit');
INSERT INTO Deliveries VALUES (3, 3, 3, '2024-11-14', 'Pending');
INSERT INTO Deliveries VALUES (4, 4, 4, '2024-11-15', 'Delivered');
INSERT INTO Deliveries VALUES (5, 5, 5, '2024-11-16', 'In Transit');

-- Step 4: Perform DDL operations
ALTER TABLE Packages ADD CONSTRAINT chk_weight CHECK (Weight > 0);
ALTER TABLE Deliveries ADD CONSTRAINT chk_status CHECK (Status IN ('Pending', 'In Transit', 'Delivered'));

-- Step 5: Perform DML operations (update, delete)
UPDATE Deliveries SET Status = 'Delivered' WHERE DeliveryID = 2;
DELETE FROM Deliveries WHERE PackageID = 5;
DELETE FROM Packages WHERE PackageID = 5;


-- Step 6: Perform DCL operations (grant and revoke permissions)
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
GRANT SELECT, INSERT ON CourierDeliverySystem.Customers TO 'user'@'localhost';

REVOKE INSERT ON CourierDeliverySystem.Customers FROM 'user'@'localhost';


-- Step 7: Perform TCL operations (transaction control)
START TRANSACTION;
UPDATE Packages SET Price = Price * 1.1 WHERE PackageID = 1;
ROLLBACK;  -- This undoes the price increase

-- Step 8: DQL operations with aggregate functions
SELECT CourierID, COUNT(*) AS DeliveryCount FROM Deliveries GROUP BY CourierID;
SELECT CustomerID, SUM(Price) AS TotalSpent FROM Packages GROUP BY CustomerID;

-- Step 9: Joins


-- Retrieve customer names, package descriptions, and courier names for all deliveries

SELECT 
    Customers.Name AS CustomerName,
    Packages.Description AS PackageDescription,
    Couriers.Name AS CourierName
FROM 
    Deliveries
INNER JOIN 
    Packages ON Deliveries.PackageID = Packages.PackageID
INNER JOIN 
    Customers ON Packages.CustomerID = Customers.CustomerID
INNER JOIN 
    Couriers ON Deliveries.CourierID = Couriers.CourierID;



-- Retrieve all deliveries, even if there is no matching customer, package, or courier
SELECT 
    Deliveries.DeliveryID,
    Customers.Name AS CustomerName,
    Packages.Description AS PackageDescription,
    Couriers.Name AS CourierName,
    Deliveries.Status
FROM 
    Deliveries
LEFT JOIN 
    Packages ON Deliveries.PackageID = Packages.PackageID
LEFT JOIN 
    Customers ON Packages.CustomerID = Customers.CustomerID
LEFT JOIN 
    Couriers ON Deliveries.CourierID = Couriers.CourierID;


-- Retrieve all packages and their delivery details, including packages that have not been delivered
SELECT 
    Packages.Description AS PackageDescription,
    Customers.Name AS CustomerName,
    Deliveries.DeliveryID,
    Couriers.Name AS CourierName,
    Deliveries.Status
FROM 
    Packages
RIGHT JOIN 
    Deliveries ON Packages.PackageID = Deliveries.PackageID
RIGHT JOIN 
    Customers ON Packages.CustomerID = Customers.CustomerID
RIGHT JOIN 
    Couriers ON Deliveries.CourierID = Couriers.CourierID;


-- Count the number of deliveries each courier has completed
SELECT 
    Couriers.Name AS CourierName,
    COUNT(Deliveries.DeliveryID) AS DeliveryCount
FROM 
    Couriers
LEFT JOIN 
    Deliveries ON Couriers.CourierID = Deliveries.CourierID
GROUP BY 
    Couriers.Name;


-- Retrieve deliveries sorted by customer name and delivery date
SELECT 
    Customers.Name AS CustomerName,
    Packages.Description AS PackageDescription,
    Couriers.Name AS CourierName,
    Deliveries.DeliveryDate
FROM 
    Deliveries
INNER JOIN 
    Packages ON Deliveries.PackageID = Packages.PackageID
INNER JOIN 
    Customers ON Packages.CustomerID = Customers.CustomerID
INNER JOIN 
    Couriers ON Deliveries.CourierID = Couriers.CourierID
ORDER BY 
    Customers.Name ASC, Deliveries.DeliveryDate DESC;

-- Retrieve customers who have more than one package
SELECT 
    c1.Name AS CustomerName, 
    p1.Description AS FirstPackageDescription, 
    p2.Description AS SecondPackageDescription
FROM 
    Packages p1
JOIN 
    Packages p2 ON p1.CustomerID = p2.CustomerID AND p1.PackageID < p2.PackageID
JOIN 
    Customers c1 ON p1.CustomerID = c1.CustomerID;




-- Step 10: Sorting and filtering
SELECT * FROM Deliveries ORDER BY DeliveryDate DESC;
SELECT * FROM Packages WHERE Price > 100 ORDER BY Price ASC;

-- Step 11: ACID properties demonstration (atomicity, consistency, isolation, durability)
START TRANSACTION;
INSERT INTO Packages VALUES (6, 'Monitor', 3.0, 150.00, 1);
INSERT INTO Deliveries VALUES (6, 6, 2, '2024-11-17', 'Pending');
COMMIT;

-- Step 12: Constraints and triggers
DELIMITER //

CREATE TRIGGER before_package_insert
BEFORE INSERT ON Packages
FOR EACH ROW
BEGIN
    IF NEW.Weight <= 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Weight must be greater than 0';
    END IF;
END //

DELIMITER ;
-- Step 13: Using clauses (WHERE, GROUP BY, HAVING)
SELECT CourierID, COUNT(*) AS DeliveryCount FROM Deliveries GROUP BY CourierID HAVING DeliveryCount > 1;
SELECT * FROM Packages WHERE Price BETWEEN 50 AND 100;

-- Step 14: Aggregate functions
SELECT AVG(Weight) AS AverageWeight FROM Packages;
SELECT MIN(Price) AS MinimumPrice, MAX(Price) AS MaximumPrice FROM Packages;

-- Step 15: Sort and limit results
SELECT * FROM Customers ORDER BY Name LIMIT 3;




