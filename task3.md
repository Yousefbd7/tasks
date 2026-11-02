SQL CODE : 
```sql
create table category (
    category_id int primary key,
    category_name varchar(25) not null
);

create table product (
    product_id int primary key,
    category_id int not null,
    name varchar(20) not null,
    description text,
    price numeric (5,2) not null,
    stock_quanttity int not null,

    foreign key (category_id) references category (category_id)
);

create table customer (
    customer_id int primary key not null,
    frist_name varchar(15) not null,
    last_name varchar(15) not null,
    email varchar(30) not null,
    passward varchar(50) not null
);

create table Order (
    order_id int PRIMARY KEY not null,
    customer_id int not null,
    order_date date not null,
    total_amount numeric (6,2),

    FOREIGN KEY (customer_id) references customer (customer_id)
);

create table order_details (
    order_detail_id int PRIMARY KEY not null,
    order_id int not null,
    product_id int not null,
    quantity int not null,
    unit_price numeric(5,2) not null,

    foreign key (order_id) references Order (order_id),
    foreign key (product_id) references product (product_id)
);
 ```
-
ERD : 
<img width="1435" height="585" alt="2- ERD" src="https://github.com/user-attachments/assets/7af11e84-8614-4958-8e0a-790fce8a6e61" />
-
3- 
```SQL
SELECT SUM(total_amount) AS dialy_total
FROM orders
WHERE order_date = '2027-11-1';
```
-
4- 
```SQL
SELECT 
    product.name, 
    SUM(order_details.quantity)
FROM order_details
JOIN product ON order_details.product_id = product.product_id
JOIN orders ON order_details.order_id = orders.order_id
GROUP BY product.name
ORDER BY SUM(order_details.quantity) DESC;
```
-
5- 
```SQL
SELECT 
    Customer.frist_name, 
    SUM(Order.total_amount)
FROM Customer
JOIN Order ON Customer.customer_id = Order.customer_id
WHERE Order.order_date >= '2026-10-01'
GROUP BY Customer.customer_id
HAVING SUM(Order.total_amount) > 500;
```
