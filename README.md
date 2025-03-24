# SQL-Project
Inventory and Finance Tracking System

## 📖 Problem Statement
Managing inventory and financial transactions is crucial for suppliers, showroom owners, and customers. Traditional systems struggle with inefficiencies in stock tracking, delayed payments, and financial 
Mis - management. 
**Inventory and Finance Tracking System** provides a structured, relational database to efficiently manage transactions, ensuring smooth and transparent business operations.

---

## 📂 Database Tables & Structure

### 1. **Supplier Table**
Stores details of suppliers providing inventory to showrooms.
```sql
CREATE TABLE supplier (
    supplier_id VARCHAR(12) PRIMARY KEY,
    name VARCHAR(15) NOT NULL,
    phone VARCHAR(12) UNIQUE NOT NULL,
    email VARCHAR(20) UNIQUE NOT NULL,
    address VARCHAR(15) NOT NULL
);
```

### 2. **Showroom Table**
Stores showroom details and owner information.
```sql
CREATE TABLE showroom (
    showroom_owner_id VARCHAR(12) PRIMARY KEY,
    name VARCHAR(20) NOT NULL,
    phone VARCHAR(12) UNIQUE NOT NULL,
    email VARCHAR(25) UNIQUE NOT NULL,
    address VARCHAR(15) NOT NULL
);
```

### 3. **Customer Table**
Stores details of customers purchasing from showrooms.
```sql
CREATE TABLE customer (
    customer_id VARCHAR(12) PRIMARY KEY,
    name VARCHAR(20) NOT NULL,
    phone VARCHAR(12) UNIQUE NOT NULL,
    email VARCHAR(20) UNIQUE NOT NULL,
    address VARCHAR(15) NOT NULL
);
```

### 4. **Inventory Table**
Stores details of inventory managed by showrooms.
```sql
CREATE TABLE showroom_inventory (
    inventory_id VARCHAR(12) PRIMARY KEY,
    showroom_owner_id VARCHAR(12) NOT NULL,
    product_name VARCHAR(20) NOT NULL,
    quantity INT NOT NULL CHECK (quantity >= 0),
    price DECIMAL(8,2) NOT NULL,
    FOREIGN KEY (showroom_owner_id) REFERENCES showroom(showroom_owner_id)
);
```

### 5. **Supplier Purchases Table**
Tracks inventory purchases from suppliers.
```sql
CREATE TABLE purchases_from_supplier (
    purchase_id VARCHAR(12) PRIMARY KEY,
    supplier_id VARCHAR(12) NOT NULL,
    inventory_id VARCHAR(12) NOT NULL,
    purchase_date DATE NOT NULL,
    quantity INT NOT NULL CHECK (quantity > 0),
    total_cost DECIMAL(10,2) NOT NULL CHECK (total_cost >= 0),
    FOREIGN KEY (supplier_id) REFERENCES supplier(supplier_id),
    FOREIGN KEY (inventory_id) REFERENCES   showroom_inventory(inventory_id)
);
```

### 6. **Customer Purchases Table**
Tracks sales made by showrooms to customers.
```sql
CREATE TABLE purchases_by_customer (
    purchases_date DATE NOT NULL,
    inventory_item VARCHAR(20) NOT NULL,
    showroom_id VARCHAR(12) NOT NULL,
    customer_id VARCHAR(12) NOT NULL,
    quantity_supplied INT NOT NULL CHECK (quantity_supplied > 0),
    totalAmount DECIMAL(10,2) NOT NULL CHECK (totalAmount >= 0),
    amount_paid DECIMAL(8,2) NOT NULL CHECK (amount_paid >= 0),
    amount_pending DECIMAL(12,2) NOT NULL CHECK (amount_pending >= 0),
    FOREIGN KEY (showroom_id) REFERENCES showroom(showroom_owner_id),
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
);
```

---

## 📊 Sample Data with Related Records

```sql
-- Suppliers
INSERT INTO supplier VALUES ('01', 'ROHIT’, '9876543210', 'rohit@supplies.com', 'NEW YORK');
INSERT INTO supplier VALUES ('02', 'ADITYA’, '8765432109', 'aditya@traders.com', 'GERMANY');

-- Showrooms
INSERT INTO showroom VALUES ('01', 'Mega Showroom', '7654321098', 'mega@showroom.com', 'MUMBAI');
INSERT INTO showroom VALUES ('02', 'Smart Electronics', '6543210987', 'smart@electronics.com', 'PUNE');

-- Customers
INSERT INTO customer VALUES ('01', 'PRAVIN', '5432109876', 'rohit@gmail.com', 'DELHI');
INSERT INTO customer VALUES ('02', 'ARVIN', '4321098765', 'rohan@gmail.com', 'SOLAPUR');

-- Inventory
INSERT INTO showroom_inventory VALUES ('INV001', '01', 'Laptop', 50, 70000.00);
INSERT INTO showroom_inventory VALUES ('INV002', '02', 'Smartphone', 100, 45000.00);

-- Purchases from Supplier
INSERT INTO purchases_from_supplier 
VALUES ('PUR001', '01', 'INV001', TO_DATE('2024-01-10', 'YYYY-MM-DD'), 30, 150000.00);

INSERT INTO purchases_from_supplier 
VALUES ('PUR002', '02', 'INV002', TO_DATE('2025-01-15', 'YYYY-MM-DD'), 50, 250000.00);


-- Purchases by Customer
INSERT INTO purchases_by_customer
VALUES (TO_DATE('2024-02-05', 'YYYY-MM-DD'), 'Laptop', '01', '01', 1, 70000.00, 60000.00, 10000.00);

INSERT INTO purchases_by_customer 
VALUES (TO_DATE('2024-02-10', 'YYYY-MM-DD'), 'Smartphone', '02', '02', 2, 45000.00, 40000.00, 5000.00);
```


--ALL TABLES:
--1. SHOWROOM TABLE:
SQL> SELECT * FROM SHOWROOM;

SHOWROOM_OWN NAME                 PHONE        EMAIL                     ADDRESS                                                                                                                        
------------ -------------------- ------------ ------------------------- ---------------                                                                                                                
01           Mega Showroom        7654321098   mega@showroom.com         MUMBAI                                                                                                                         
02           Smart Electronics    6543210987   smart@electronics.com     PUNE                                                                                                                           

2 rows selected.



--2. SUPPLIER TABLE:
SQL> SELECT * FROM SUPPLIER;

SUPPLIER_ID  NAME            PHONE        EMAIL                ADDRESS                                                                                                                                  
------------ --------------- ------------ -------------------- ---------------                                                                                                                          
01           ROHIT           9876543210   rohit@supplies.com   NEW YORK                                                                                                                                 
02           ADITYA          8765432109   aditya@traders.com   GERMANY                                                                                                                                  

2 rows selected.

--3.CUSTOMER TABLE:
SQL> SELECT * FROM CUSTOMER;

CUSTOMER_ID  NAME                 PHONE        EMAIL                ADDRESS                                                                                                                             
------------ -------------------- ------------ -------------------- ---------------                                                                                                                     
01           PRAVIN               5432109876   rohit@gmail.com      DELHI                                                                                                                               
02           ARVIN                4321098765   rohan@gmail.com      SOLAPUR                                                                                                                             

2 rows selected.


--4. SHOWROOM INVENTORY:
SQL> SELECT * FROM SHOWROOM_INVENTORY;

INVENTORY_ID SHOWROOM_OWN PRODUCT_NAME           QUANTITY      PRICE                                                                                                                                    
------------ ------------ -------------------- ---------- ----------                                                                                                                                    
INV001       01           Laptop                       50      70000                                                                                                                                    
INV002       02           Smartphone                  100      45000                                                                                                                                    

2 rows selected.

--5. PURCHASES FROM SUPPLIER
SQL> SELECT * FROM PURCHASES_FROM_SUPPLIER;

PURCHASE_ID  SUPPLIER_ID  INVENTORY_ID PURCHASE_   QUANTITY TOTAL_COST                                                                                                                                  
------------ ------------ ------------ --------- ---------- ----------                                                                                                                                  
PUR001       01           INV001       10-JAN-24         30     150000                                                                                                                                  
PUR002       02           INV002       15-JAN-25         50     250000                                                                                                                                  

2 rows selected.

--6. PURCHASES BY CUSTOMER:
SQL> SELECT * FROM PURCHASES_BY_CUSTOMER;

PURCHASES INVENTORY_ITEM       SHOWROOM_ID  CUSTOMER_ID  QUANTITY_SUPPLIED TOTALAMOUNT AMOUNT_PAID AMOUNT_PENDING                                                                                       
--------- -------------------- ------------ ------------ ----------------- ----------- ----------- --------------                                                                                       
05-FEB-24 Laptop               01           01                           1       70000       60000          10000                                                                                       
10-FEB-24 Smartphone           02           02                           2       45000       40000           5000                                                                                       

2 rows selected.
------------------------------------------------

##   MySQL Business Queries

```sql
-- 1. Total quantity of inventory supplied by each supplier
SELECT supplier_id, SUM(quantity) AS total_supplied FROM purchases_from_supplier GROUP BY supplier_id;

-- 2. Total sales amount showroom-wise
SELECT showroom_id, SUM(totalAmount) AS total_sales FROM purchases_by_customer GROUP BY showroom_id;



-- 3. Most sold product of the year
SELECT * FROM (
    SELECT inventory_item, SUM(quantity_supplied) AS total_sold 
    FROM purchases_by_customer
    WHERE EXTRACT(YEAR FROM purchases_date) = EXTRACT(YEAR FROM SYSDATE)
    GROUP BY inventory_item 
    ORDER BY total_sold DESC
) WHERE ROWNUM = 1;


-- 4. Total due amount showroom-wise
SELECT showroom_id, SUM(amount_pending) AS total_due 
FROM purchases_by_customer
GROUP BY showroom_id;

-- 5. Customers with highest total purchases
SELECT * FROM (
    SELECT customer_id, SUM(totalAmount) AS total_spent 
    FROM purchases_by_customer
    GROUP BY customer_id 
    ORDER BY total_spent DESC
) WHERE ROWNUM = 1;

---

## 🚀 **Conclusion**
The **Inventory and Finance Tracking System** ensures a structured, efficient, and scalable approach to **inventory management and financial tracking**. It provides transparency, accurate record-keeping, and better decision-making for suppliers, showroom owners, and customers. 📊✅

