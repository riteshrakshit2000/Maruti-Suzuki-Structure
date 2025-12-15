--  drop database maruti_suzuki_db;
create database if not exists maruti_suzuki_db;
use maruti_suzuki_db;
create table if not exists customers(
 customer_id int unsigned auto_increment primary key not null,
    customer_first_name varchar(50) not null,
    customer_last_name varchar (50) not null,
    customer_DOB date not null,
    customer_contact_number varchar (10) not null,
    email VARCHAR(100) UNIQUE NOT NULL,
    city VARCHAR(50) NOT NULL,
    address TEXT,
    registration_date DATE NOT NULL,
    customer_type ENUM('Individual', 'Corporate') DEFAULT 'Individual',
   constraint chek_phone_length check (char_length(customer_contact_number)=10)
);

create table if not exists equipment(
equipment_id int unsigned auto_increment primary key,
 equipment_code varchar(10) not null unique ,
equipment_name varchar(10) not null unique 

);

create table if not exists supplier(
supplier_id int unsigned auto_increment primary key,

contact_no varchar(100) not null ,
equipment_id int unsigned not null ,
foreign key (equipment_id) references equipment(equipment_id) on delete cascade,
email varchar (20) unique not null,
contact_name varchar(20) not null,
city varchar(50),
constraint chk_phone_length check (char_length(contact_no)=10),

    
    
    
    
    
 component_type VARCHAR(100),
    payment_terms VARCHAR(100),
    rating DECIMAL(3,2),
   
    active_status ENUM('Active', 'Inactive') DEFAULT 'Active'
);

create table if not exists vehicle_models (
    model_id int unsigned auto_increment primary key,
    model_name varchar(100) NOT NULL UNIQUE,
    segment varchar(50),
    price varchar(10),
    mfg_year year
);
CREATE TABLE if not exists VARIANTS (
    variant_id INT PRIMARY KEY AUTO_INCREMENT,
    model_id INT unsigned NOT NULL,
    variant_name VARCHAR(100) NOT NULL,
    fuel_type VARCHAR(20),
    transmission VARCHAR(20),
    price DECIMAL(10,2) NOT NULL,
  
    FOREIGN KEY (model_id) REFERENCES VEHICLE_MODELS(model_id) ON DELETE CASCADE,
    UNIQUE KEY unique_variant (model_id, variant_name)
);

CREATE TABLE if not exists DEALERSHIPS (
    dealership_id INT unsigned PRIMARY KEY AUTO_INCREMENT,
    dealership_name VARCHAR(100) NOT NULL UNIQUE,
    model_id INT unsigned NOT NULL,
    city VARCHAR(50) NOT NULL,
    address TEXT,
    phone VARCHAR(15) NOT NULL,
    email VARCHAR(100),
    manager_name VARCHAR(100),
    established_year YEAR,
   
    FOREIGN KEY (model_id) REFERENCES VEHICLE_MODELS(model_id) ON DELETE RESTRICT
);

-- 5. NEW TABLE: DEPARTMENTS
CREATE TABLE if not exists DEPARTMENTS (
    department_id INT PRIMARY KEY AUTO_INCREMENT,
    department_name VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    head_name VARCHAR(100),
    budget DECIMAL(12,2)
   
);

-- 6. EMPLOYEES (Now linked to DEPARTMENTS)
CREATE TABLE if not exists EMPLOYEES (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    dealership_id INT unsigned NOT NULL,
    department_id INT NOT NULL,
    employee_name VARCHAR(100) NOT NULL,
    role VARCHAR(50) NOT NULL,
    email VARCHAR(100),
    phone VARCHAR(15),
    salary DECIMAL(10,2),
    hire_date DATE,
    status ENUM('Active', 'Inactive') DEFAULT 'Active',
  
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIPS(dealership_id) ON DELETE CASCADE,
    FOREIGN KEY (department_id) REFERENCES DEPARTMENTS(department_id) ON DELETE RESTRICT
);


-- SALES & TRANSACTION TABLES (3 Tables)


-- 7. SALES_ORDERS (PRIMARY LINK TO CUSTOMERS)
CREATE TABLE if not exists SALES_ORDERS (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT unsigned NOT NULL,
    dealership_id  INT unsigned NOT NULL,
    variant_id INT  NOT NULL,
    order_date DATE NOT NULL,
    delivery_date DATE,
    total_amount DECIMAL(12,2) NOT NULL,
    discount_amount DECIMAL(10,2) DEFAULT 0,
    tax_amount DECIMAL(10,2) DEFAULT 0,
    final_amount DECIMAL(12,2) NOT NULL,
    order_status ENUM('Pending', 'Confirmed', 'Delivered', 'Cancelled') DEFAULT 'Pending',
 
    FOREIGN KEY (customer_id) REFERENCES CUSTOMERS(customer_id) ON DELETE RESTRICT,
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIPS(dealership_id) ON DELETE RESTRICT,
    FOREIGN KEY (variant_id) REFERENCES VARIANTS(variant_id) ON DELETE RESTRICT,
    INDEX idx_customer_id (customer_id),
    INDEX idx_order_date (order_date)
);

-- 8. INVENTORY (Now linked to SALES_ORDERS)
CREATE TABLE if not exists INVENTORY (
    inventory_id INT PRIMARY KEY AUTO_INCREMENT,
    dealership_id INT unsigned NOT NULL,
    variant_id INT NOT NULL,
    order_id INT,
    stock_quantity INT DEFAULT 0,
    reorder_level INT DEFAULT 5,
    max_stock INT DEFAULT 50,
  
    FOREIGN KEY (dealership_id) REFERENCES DEALERSHIPS(dealership_id) ON DELETE CASCADE,
    FOREIGN KEY (variant_id) REFERENCES VARIANTS(variant_id) ON DELETE CASCADE,
    FOREIGN KEY (order_id) REFERENCES SALES_ORDERS(order_id) ON DELETE SET NULL,
    UNIQUE KEY unique_inventory (dealership_id, variant_id)
);

-- 9. PAYMENTS (LINKED TO CUSTOMERS VIA SALES_ORDERS)
CREATE TABLE if not exists PAYMENTS (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    payment_date DATE NOT NULL,
    payment_method VARCHAR(50) NOT NULL,
    amount_paid DECIMAL(12,2) NOT NULL,
    payment_status ENUM('Pending', 'Completed', 'Failed', 'Refunded') DEFAULT 'Pending',
    transaction_id VARCHAR(100),
    notes TEXT,
  
    FOREIGN KEY (order_id) REFERENCES SALES_ORDERS(order_id) ON DELETE RESTRICT,
    INDEX idx_order_id (order_id),
    INDEX idx_payment_date (payment_date)
);


-- VEHICLE & SERVICE TABLES (3 Tables)


-- 10. VEHICLES (LINKED TO CUSTOMERS VIA SALES_ORDERS)
CREATE TABLE if not exists VEHICLES (
    vehicle_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL UNIQUE,
    chassis_number VARCHAR(50) NOT NULL UNIQUE,
    engine_number VARCHAR(50) NOT NULL UNIQUE,
    registration_number VARCHAR(20) NOT NULL UNIQUE,
    manufacture_date DATE NOT NULL,
    color VARCHAR(30),
    fuel_tank_capacity DECIMAL(5,2),
    engine_displacement INT,
    seating_capacity INT,
   
    FOREIGN KEY (order_id) REFERENCES SALES_ORDERS(order_id) ON DELETE RESTRICT,
    INDEX idx_chassis (chassis_number),
    INDEX idx_registration (registration_number)
);

-- 11. SERVICE_CENTERS
CREATE TABLE SERVICE_CENTERS (
    center_id INT PRIMARY KEY AUTO_INCREMENT,
    center_name VARCHAR(100) NOT NULL UNIQUE,
    city VARCHAR(50) NOT NULL,
    address TEXT,
    phone VARCHAR(15) NOT NULL,
    email VARCHAR(100),
    manager_name VARCHAR(100),
    working_hours VARCHAR(50),
    established_year YEAR
  
);

-- 12. SERVICE_RECORDS (LINKED TO CUSTOMERS VIA VEHICLES)
CREATE TABLE SERVICE_RECORDS (
    service_id INT PRIMARY KEY AUTO_INCREMENT,
    vehicle_id INT NOT NULL,
    center_id INT NOT NULL,
    service_date DATE NOT NULL,
    service_type VARCHAR(50) NOT NULL,
    description TEXT,
    cost DECIMAL(10,2),
    parts_cost DECIMAL(10,2),
    labor_cost DECIMAL(10,2),
    odometer_reading INT,
    service_status ENUM('Completed', 'Pending', 'In Progress') DEFAULT 'Completed',
    next_service_due DATE,
  
    FOREIGN KEY (vehicle_id) REFERENCES VEHICLES(vehicle_id) ON DELETE CASCADE,
    FOREIGN KEY (center_id) REFERENCES SERVICE_CENTERS(center_id) ON DELETE RESTRICT,
    INDEX idx_vehicle_id (vehicle_id),
    INDEX idx_service_date (service_date)
);


-- SUPPLY CHAIN & WARRANTY TABLES (5 Tables)




-- 14. COMPONENTS
CREATE TABLE if not exists COMPONENTS (
    component_id INT PRIMARY KEY AUTO_INCREMENT,
    supplier_id INT unsigned NOT NULL,
    component_name VARCHAR(100) NOT NULL,
    component_code VARCHAR(50) NOT NULL UNIQUE,
    category VARCHAR(50),
    unit_cost DECIMAL(10,2) NOT NULL,
    stock_on_hand INT DEFAULT 0,
    reorder_level INT DEFAULT 10,
    last_reorder_date DATE,
  
    FOREIGN KEY (supplier_id) REFERENCES supplier(supplier_id) ON DELETE RESTRICT,
    INDEX idx_component_code (component_code)
);

-- 15. PURCHASE_ORDERS
CREATE TABLE if not exists PURCHASE_ORDERS (
    po_id INT PRIMARY KEY AUTO_INCREMENT,
    supplier_id INT unsigned NOT NULL,
    component_id INT NOT NULL,
    po_date DATE NOT NULL,
    quantity INT NOT NULL,
    unit_cost DECIMAL(10,2) NOT NULL,
   --  total_cost DECIMAL(12,2) GENERATED ALWAYS AS (quantity * unit_cost) STORED,
    expected_delivery_date DATE,
    actual_delivery_date DATE,
    delivery_status ENUM('Pending', 'Delivered', 'Partial', 'Cancelled') DEFAULT 'Pending',
    payment_status ENUM('Unpaid', 'Paid', 'Partial') DEFAULT 'Unpaid',
    notes TEXT,

    FOREIGN KEY (supplier_id) REFERENCES supplier(supplier_id) ON DELETE RESTRICT,
    FOREIGN KEY (component_id) REFERENCES COMPONENTS(component_id) ON DELETE RESTRICT,
    INDEX idx_po_date (po_date),
    INDEX idx_delivery_status (delivery_status)
);

-- 16. WARRANTY (NOW linked to SALES_ORDERS AND PURCHASE_ORDERS)
CREATE TABLE if not exists WARRANTY (
    warranty_id INT PRIMARY KEY AUTO_INCREMENT,
    vehicle_id INT  NOT NULL,
    sales_order_id INT  NOT NULL,
    purchase_order_id INT ,
    warranty_start_date DATE NOT NULL,
    warranty_end_date DATE NOT NULL,
    coverage_type VARCHAR(50) NOT NULL,
    coverage_km INT,
    claim_count INT DEFAULT 0,
    max_claims_allowed INT DEFAULT 5,
    warranty_status ENUM('Active', 'Expired', 'Claimed') DEFAULT 'Active',
    notes TEXT,
  
    FOREIGN KEY (vehicle_id) REFERENCES VEHICLES(vehicle_id) ON DELETE CASCADE,
    FOREIGN KEY (sales_order_id) REFERENCES SALES_ORDERS(order_id) ON DELETE RESTRICT,
    FOREIGN KEY (purchase_order_id) REFERENCES PURCHASE_ORDERS(po_id) ON DELETE SET NULL,
    INDEX idx_warranty_status (warranty_status)
);

-- MARUTI SUZUKI DATABASE - INSERT STATEMENTS

INSERT INTO customers (customer_first_name, customer_last_name, customer_DOB, customer_contact_number, email, city, address, registration_date, customer_type) VALUES
('Rajesh', 'Kumar', '1985-05-15', '9876543210', 'rajesh.kumar@email.com', 'Mumbai', '123 Marine Drive, Mumbai', '2025-01-10', 'Individual');


INSERT INTO equipment (equipment_code, equipment_name) VALUES
('EQ001', 'Engine');


INSERT INTO supplier (contact_no, equipment_id, email, contact_name, city, component_type, payment_terms, rating, active_status) VALUES
('9876543220', 1, 'supplier1@abc.com', 'Rajat Gupta', 'Surat', 'Engine Parts', '30 days', 4.50, 'Active');


INSERT INTO vehicle_models (model_name, segment, price, mfg_year) VALUES
('Alto', 'Hatchback', '320000', 2025);

INSERT INTO variants (model_id, variant_name, fuel_type, transmission, price) VALUES
(1, 'Alto LXi', 'Petrol', 'Manual', 320000.00);


INSERT INTO dealerships (dealership_name, model_id, city, address, phone, email, manager_name, established_year) VALUES
('Maruti Mumbai Downtown', 1, 'Mumbai', '123 Bandra West, Mumbai', '02226456789', 'mumbai.downtown@maruti.com', 'Rajesh Kumar', 2010);

-- ============================================
-- 7. DEPARTMENTS TABLE
-- ============================================
INSERT INTO departments (department_name, description, head_name, budget) VALUES
('Sales', 'Customer sales and order management', 'Rajesh Kumar', 5000000.00);

INSERT INTO employees (dealership_id, department_id, employee_name, role, email, phone, salary, hire_date, status) VALUES
(1, 1, 'Sandeep Kumar', 'Sales Executive', 'sandeep.kumar@maruti.com', '9876543228', 45000.00, '2023-01-15', 'Active');


INSERT INTO sales_orders (customer_id, dealership_id, variant_id, order_date, delivery_date, total_amount, discount_amount, tax_amount, final_amount, order_status) VALUES
(1, 1, 1, '2025-01-10', '2025-02-01', 320000.00, 15000.00, 45000.00, 350000.00, 'Delivered');


INSERT INTO inventory (dealership_id, variant_id, order_id, stock_quantity, reorder_level, max_stock) VALUES
(1, 1, 1, 8, 5, 50);


INSERT INTO payments (order_id, payment_date, payment_method, amount_paid, payment_status, transaction_id, notes) VALUES
(1, '2025-01-15', 'Bank Transfer', 350000.00, 'Completed', 'TXN20250115001', 'Full payment received');


INSERT INTO vehicles (order_id, chassis_number, engine_number, registration_number, manufacture_date, color, fuel_tank_capacity, engine_displacement, seating_capacity) VALUES
(1, 'CHASS0000001', 'ENG0000001', 'MH0120250001', '2025-01-05', 'Silver', 35.00, 796, 5);

INSERT INTO service_centers (center_name, city, address, phone, email, manager_name, working_hours, established_year) VALUES
('Maruti Service Mumbai', 'Mumbai', '123 Thane East, Mumbai', '02225671234', 'service.mumbai@maruti.com', 'Rajesh Desai', '8:00 AM - 6:00 PM', 2008);


INSERT INTO service_records (vehicle_id, center_id, service_date, service_type, description, cost, parts_cost, labor_cost, odometer_reading, service_status, next_service_due) VALUES
(1, 1, '2025-02-15', 'Regular Service', 'Oil change, filter replacement', 4500.00, 2000.00, 2500.00, 5000, 'Completed', '2025-05-15');


INSERT INTO components (supplier_id, component_name, component_code, category, unit_cost, stock_on_hand, reorder_level, last_reorder_date) VALUES
(1, 'Engine Oil', 'COMP001', 'Lubricants', 150.00, 500, 100, '2025-02-01');


INSERT INTO purchase_orders (supplier_id, component_id, po_date, quantity, unit_cost, expected_delivery_date, actual_delivery_date, delivery_status, payment_status, notes) VALUES
(1, 1, '2025-01-20', 500, 150.00, '2025-02-05', '2025-02-04', 'Delivered', 'Paid', 'Engine oil bulk order');

INSERT INTO warranty (vehicle_id, sales_order_id, purchase_order_id, warranty_start_date, warranty_end_date, coverage_type, coverage_km, claim_count, max_claims_allowed, warranty_status, notes) VALUES
(1, 1, NULL, '2025-02-01', '2027-02-01', 'Factory', 100000, 0, 5, 'Active', '2-year comprehensive warranty');


