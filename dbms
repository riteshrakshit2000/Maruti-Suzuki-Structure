--  drop database maruti_suzuki_db;
create database if not exists maruti_suzuki_db;
use maruti_suzuki_db;
create table if not exists customers(
 customer_id int unsigned auto_increment primary key not null,
    customer_first_name varchar(50) not null,
    customer_last_name varchar(50) not null,
    customer_DOB date not null,
    customer_contact_number varchar(10) not null,
    email varchar(100) unique not null,
    city varchar(50) not null,
    address text,
    registration_date date not null,
    customer_type enum('Individual', 'Corporate') default 'Individual',
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

    
    
    
    
    
 component_type varchar(100),
    payment_terms varchar(100),
    rating decimal(3,2),
   
    active_status enum('Active', 'Inactive') default 'Active'
);

create table if not exists vehicle_models (
    model_id int unsigned auto_increment primary key,
    model_name varchar(100) not null unique,
    segment varchar(50),
    price varchar(10),
    mfg_year year
);
create table if not exists variants (
    variant_id int primary key auto_increment,
    model_id int unsigned not null,
    variant_name varchar(100) not null,
    fuel_type varchar(20),
    transmission varchar(20),
    price decimal(10,2) not null,
  
    foreign key (model_id) references vehicle_models(model_id) on delete cascade,
    unique key unique_variant (model_id, variant_name)
);

create table if not exists dealerships (
    dealership_id int unsigned primary key auto_increment,
    dealership_name varchar(100) not null unique,
    model_id int unsigned not null,
    city varchar(50) not null,
    address TEXT,
    phone varchar(15) not null,
    email varchar(100),
    manager_name varchar(100),
    established_year year,
   
    foreign key (model_id) references vehicle_models(model_id) on delete restrict
);


create table if not exists departments (
    department_id int primary key auto_increment,
    department_name varchar(100) not null unique,
    description text,
    head_name varchar(100),
    budget decimal(12,2)
   
);


create table if not exists employees (
    employee_id int primary key auto_increment,
    dealership_id int unsigned not null ,
    department_id int not null ,
    employee_name varchar(100) not null,
    role varchar(50) not null,
    email varchar(100),
    phone varchar(15),
    salary decimal(10,2),
    hire_date date,
    status enum('Active', 'Inactive') default 'Active',
  
   foreign key (dealership_id) references dealerships(dealership_id) on delete cascade,
   foreign key (department_id) references departments(department_id) on delete restrict
);


create table if not exists sales_orders (
    order_id int primary key auto_increment,
    customer_id int unsigned not null,
    dealership_id int unsigned not null,
    variant_id int not null,
    order_date date not null,
    delivery_date date,
    total_amount decimal(12,2) not null,
    discount_amount decimal(10,2) default 0,
    tax_amount decimal(10,2) default 0,
    final_amount decimal(12,2) not null,
    order_status enum('Pending', 'Confirmed', 'Delivered', 'Cancelled') default 'Pending',
 
    foreign key (customer_id) references customers(customer_id) on delete restrict,
    foreign key (dealership_id) references dealerships(dealership_id) on delete restrict,
    foreign key (variant_id) references variants(variant_id) on delete restrict
   
);


create table if not exists inventory (
    inventory_id int primary key auto_increment,
    dealership_id int unsigned not null,
    variant_id int not null,
    order_id int,
    stock_quantity int default 0,
    reorder_level int default 5,
    max_stock int default 50,
  
    foreign key (dealership_id) references dealerships(dealership_id) on delete cascade,
    foreign key (variant_id) references variants(variant_id) on delete cascade,
   foreign key (order_id) references sales_orders(order_id) on delete set null,
    unique key unique_inventory (dealership_id, variant_id)
);


create table if not exists payments (
    payment_id int primary key auto_increment,
    order_id int not null,
    payment_date date not null,
    payment_method varchar(50) not null,
    amount_paid decimal(12,2) not null,
    payment_status enum('Pending', 'Completed', 'Failed', 'Refunded') default 'Pending',
    transaction_id varchar(100),
    notes text,
  
   foreign key (order_id) references sales_orders(order_id) on delete restrict
  
);


create table if not exists vehicles(
    vehicle_id int primary key auto_increment,
    order_id int not null unique,
    chassis_number varchar(50) not null unique,
    engine_number varchar(50) not null unique,
    registration_number varchar(20) not null unique,
    manufacture_date date not null,
    color varchar(30),
    fuel_tank_capacity decimal(5,2),
    engine_displacement int,
    seating_capacity int,
   
   foreign key (order_id) references sales_orders(order_id) on delete restrict
  
);


create table if not exists service_centers (
    center_id int primary key auto_increment,
    center_name varchar(100) not null unique ,
    city varchar(50) not null,
    address text,
    phone varchar(15) not null,
    email varchar(100),
    manager_name varchar(100),
    working_hours varchar(50),
    established_year year
  
);

create table if not exists service_records (
    service_id int primary key auto_increment,
    vehicle_id int not null,
    center_id int not null,
    service_date date not null,
    service_type varchar(50) not null,
    description text,
    cost decimal(10,2),
    parts_cost decimal(10,2),
    labor_cost decimal(10,2),
    odometer_reading int,
    service_status enum('Completed', 'Pending', 'In Progress') default 'Completed',
    next_service_due date,
  
   foreign key (vehicle_id) references vehicles(vehicle_id) on delete CASCADE,
   foreign key (center_id) references service_centers(center_id) on delete RESTRICT
   
);


create table if not exists components (
    component_id int primary key auto_increment,
    supplier_id int unsigned not null,
    component_name varchar(100) not null,
    component_code varchar(50) not null unique,
    category varchar(50),
    unit_cost decimal(10,2) not null,
    stock_on_hand int default 0,
    reorder_level int default 10,
    last_reorder_date date,
  
    foreign key (supplier_id) references supplier(supplier_id) on delete RESTRICT
   
);


create table if not exists purchase_orders (
    po_id int primary key auto_increment,
    supplier_id int unsigned not null,
    component_id int  not null,
    po_date date  not null,
    quantity int  not null,
    unit_cost decimal(10,2)  not null,
   --  total_cost DECIMAL(12,2) GENERATED ALWAYS AS (quantity * unit_cost) STORED,
    expected_delivery_date date,
    actual_delivery_date date,
    delivery_status enum('Pending', 'Delivered', 'Partial', 'Cancelled') default 'Pending',
    payment_status enum('Unpaid', 'Paid', 'Partial') default 'Unpaid',
    notes TEXT,

    foreign key (supplier_id) references supplier(supplier_id) on delete restrict,
    foreign key (component_id) references components(component_id) on delete restrict
   
);


create table if not exists warranty (
    warranty_id int primary key auto_increment,
    vehicle_id int  not null,
    sales_order_id int  not null,
    purchase_order_id int ,
    warranty_start_date date  not null,
    warranty_end_date date  not null,
    coverage_type varchar(50)  not null,
    coverage_km int,
    claim_count int default 0,
    max_claims_allowed int default 5,
    warranty_status enum('Active', 'Expired', 'Claimed') default 'Active',
    notes text,
  
    foreign key (vehicle_id) references vehicles(vehicle_id) on delete cascade,
    foreign key (sales_order_id) references sales_orders(order_id) on delete restrict,
    foreign key (purchase_order_id) references purchase_orders(po_id) on delete set null
   
);

--  INSERT STATEMENTS

insert into customers (customer_first_name, customer_last_name, customer_DOB, customer_contact_number, email, city, address, registration_date, customer_type) VALUES
('Rajesh', 'Kumar', '1985-05-15', '9876543210', 'rajesh.kumar@email.com', 'Mumbai', '123 Marine Drive, Mumbai', '2025-01-10', 'Individual');


insert into equipment (equipment_code, equipment_name) VALUES
('EQ001', 'Engine');


insert into supplier (contact_no, equipment_id, email, contact_name, city, component_type, payment_terms, rating, active_status) VALUES
('9876543220', 1, 'supplier1@abc.com', 'Rajat Gupta', 'Surat', 'Engine Parts', '30 days', 4.50, 'Active');


insert into vehicle_models (model_name, segment, price, mfg_year) VALUES
('Alto', 'Hatchback', '320000', 2025);

insert into variants (model_id, variant_name, fuel_type, transmission, price) VALUES
(1, 'Alto LXi', 'Petrol', 'Manual', 320000.00);


insert into dealerships (dealership_name, model_id, city, address, phone, email, manager_name, established_year) VALUES
('Maruti Mumbai Downtown', 1, 'Mumbai', '123 Bandra West, Mumbai', '02226456789', 'mumbai.downtown@maruti.com', 'Rajesh Kumar', 2010);

insert into departments (department_name, description, head_name, budget) VALUES
('Sales', 'Customer sales and order management', 'Rajesh Kumar', 5000000.00);

insert into employees (dealership_id, department_id, employee_name, role, email, phone, salary, hire_date, status) VALUES
(1, 1, 'Sandeep Kumar', 'Sales Executive', 'sandeep.kumar@maruti.com', '9876543228', 45000.00, '2023-01-15', 'Active');


insert into sales_orders (customer_id, dealership_id, variant_id, order_date, delivery_date, total_amount, discount_amount, tax_amount, final_amount, order_status) VALUES
(1, 1, 1, '2025-01-10', '2025-02-01', 320000.00, 15000.00, 45000.00, 350000.00, 'Delivered');


insert into inventory (dealership_id, variant_id, order_id, stock_quantity, reorder_level, max_stock) VALUES
(1, 1, 1, 8, 5, 50);


insert into payments (order_id, payment_date, payment_method, amount_paid, payment_status, transaction_id, notes) VALUES
(1, '2025-01-15', 'Bank Transfer', 350000.00, 'Completed', 'TXN20250115001', 'Full payment received');


insert into vehicles (order_id, chassis_number, engine_number, registration_number, manufacture_date, color, fuel_tank_capacity, engine_displacement, seating_capacity) VALUES
(1, 'CHASS0000001', 'ENG0000001', 'MH0120250001', '2025-01-05', 'Silver', 35.00, 796, 5);

insert into service_centers (center_name, city, address, phone, email, manager_name, working_hours, established_year) VALUES
('Maruti Service Mumbai', 'Mumbai', '123 Thane East, Mumbai', '02225671234', 'service.mumbai@maruti.com', 'Rajesh Desai', '8:00 AM - 6:00 PM', 2008);


insert into service_records (vehicle_id, center_id, service_date, service_type, description, cost, parts_cost, labor_cost, odometer_reading, service_status, next_service_due) VALUES
(1, 1, '2025-02-15', 'Regular Service', 'Oil change, filter replacement', 4500.00, 2000.00, 2500.00, 5000, 'Completed', '2025-05-15');


insert into components (supplier_id, component_name, component_code, category, unit_cost, stock_on_hand, reorder_level, last_reorder_date) VALUES
(1, 'Engine Oil', 'COMP001', 'Lubricants', 150.00, 500, 100, '2025-02-01');


insert into purchase_orders (supplier_id, component_id, po_date, quantity, unit_cost, expected_delivery_date, actual_delivery_date, delivery_status, payment_status, notes) VALUES
(1, 1, '2025-01-20', 500, 150.00, '2025-02-05', '2025-02-04', 'Delivered', 'Paid', 'Engine oil bulk order');

insert into warranty (vehicle_id, sales_order_id, purchase_order_id, warranty_start_date, warranty_end_date, coverage_type, coverage_km, claim_count, max_claims_allowed, warranty_status, notes) VALUES
(1, 1, NULL, '2025-02-01', '2027-02-01', 'Factory', 100000, 0, 5, 'Active', '2-year comprehensive warranty');


