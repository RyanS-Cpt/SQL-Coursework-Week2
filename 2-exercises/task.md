# E-Commerce Database

In this homework, you are going to work with an ecommerce database. In this database, you have `products` that `consumers` can buy from different `suppliers`. Customers can create an `order` and several products can be added in one order.

## Submission

Below you will find a set of tasks for you to complete to set up a database for an e-commerce app.

To submit this homework write the correct commands for each question here:
```sql


```

When you have finished all of the questions - open a pull request with your answers to the `Databases-Homework` repository.

## Setup

To prepare your environment for this homework, open a terminal and create a new database called `cyf_ecommerce`:

```sql
createdb cyf_ecommerce
```

Import the file [`cyf_ecommerce.sql`](./cyf_ecommerce.sql) in your newly created database:

```sql
psql -d cyf_ecommerce -f cyf_ecommerce.sql
```

Open the file `cyf_ecommerce.sql` in VSCode and examine the SQL code. Take a piece of paper and draw the database with the different relationships between tables (as defined by the REFERENCES keyword in the CREATE TABLE commands). Identify the foreign keys and make sure you understand the full database schema.

## Task          

Once you understand the database that you are going to work with, solve the following challenge by writing SQL queries using everything you learned about SQL:

1. Retrieve all the customers' names and addresses who live in the United States
```sql
select name, address from customers
where country ='United States';

```
2. Retrieve all the customers in ascending name sequence
```sql
select * from customers
order by name asc --(default return is asc);

```
3. Retrieve all the products whose name contains the word `socks`
```sql
select * from products 
where product_name like '%socks%';

```
4. Retrieve all the products which cost more than 100 showing product id, name, unit price and supplier id.
```sql
select product_id, product_name, unit_price, supplier_id
from order_items
inner join product_availability 
on product_id = product_availability.prod_id
inner join products 
on prod_id = products.id
where unit_price > 100;

```
5. Retrieve the 5 most expensive products
```sql
select *
from order_items 
inner join product_availability 
on product_id = product_availability.prod_id
order by unit_price desc
limit 5;

```
6. Retrieve all the products with their corresponding suppliers. The result should only contain the columns `product_name`, `unit_price` and `supplier_name`
```sql
select product_name, supplier_name, unit_price 
from product_availability 
inner join products 
on prod_id = products.id 
inner join suppliers 
on supp_id = suppliers.id;

```
7. Retrieve all the products sold by suppliers based in the United Kingdom. The result should only contain the columns `product_name` and `supplier_name`.
```sql
select product_name, supplier_name
from product_availability 
inner join products 
on prod_id = products.id 
inner join suppliers 
on supp_id = suppliers.id
where country = 'United Kingdom';

```
8. Retrieve all orders, including order items, from customer ID `1`. Include order id, reference, date and total cost (calculated as quantity * unit price).
```sql
select order_id,product_name, order_reference, order_date, (quantity * unit_price) as total_cost
from order_items 
inner join orders
on order_id = orders.id
inner join product_availability
on product_id = prod_id
inner join products 
on prod_id = products.id
where customer_id = 1;

```
9. Retrieve all orders, including order items, from customer named `Hope Crosby`
```sql
select product_name, order_id
from order_items 
inner join product_availability 
on product_id = prod_id
inner join products
on prod_id = products.id 
inner join orders 
on order_id = orders.id
inner join customers 
on customer_id = customers.id 
where name = 'Hope Crosby';

```
10. Retrieve all the products in the order `ORD006`. The result should only contain the columns `product_name`, `unit_price` and `quantity`.
```sql
select product_name, unit_price, quantity, 
from order_items 
inner join product_availability 
on product_id = prod_id
inner join products 
on prod_id = products.id
inner join orders 
on order_id = orders.id 
where order_reference = 'ORD006';

```
11. Retrieve all the products with their supplier for all orders of all customers. The result should only contain the columns `name` (from customer), `order_reference`, `order_date`, `product_name`, `supplier_name` and `quantity`.
```sql
select name, order_reference, order_date, product_name, supplier_name, quantity
from order_items 
inner join product_availability 
on product_id = prod_id
inner join products 
on prod_id = products.id
inner join suppliers
on supp_id = suppliers.id 
inner join orders 
on order_id = orders.id 
inner join customers 
on customer_id = customers.id;

```
12. Retrieve the names of all customers who bought a product from a supplier based in China.
```sql
select name
from order_items
inner join product_availability 
on supplier_id = supp_id
inner join suppliers 
on supp_id = suppliers.id 
inner join orders 
on order_id = orders.id 
inner join customers 
on customer_id = customers.id 
where suppliers.country = 'China';

```
13. List all orders giving customer name, order reference, order date and order total amount (quantity * unit price) in descending order of total.
```sql
select name, order_reference, order_date, (quantity * unit_price) as total_amount
from order_items 
inner join product_availability 
on product_id = prod_id
inner join orders 
on order_id = orders.id 
inner join customers 
on customer_id = customers.id 
order by total_amount desc;

```

