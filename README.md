# MAVEN-STORE-PROJECT WITH ANALYSIS SQL
Sales and inventory data analysis for a fictitious chain of toy store in Mexico called Maven Toys, including information about products, store, daily store transaction, and current inventory levels at each location. The Analysis of this project will be done in two parts and this is the first part. Here we will get familiar with the data set and the SQL codes for analysis, Enjoy this ride with me.

#### TOOL: MySQL 

#### QUESTIONS:
1. Which product category drive the biggest profits? is it the same across the store locations?
2. How much money is tied up in inventory at the toy stores? How long will it last?
 3. Are sales being lost with out-of-stock products at certain location?

#### UNDERSTANDING THE DATA:




#### CODES:
1. The Products table with the Alias p, Store table with Alias s and Inventory table with the Alias I were combined using the left and right join using a common column(primary and secondary keys).
The group by function and order by function.

```SQL
select
	p.product_id,
	p.product_name,
	p.product_category,
	s.store_id,
	s.store_location,
max((product_price-product_cost)) as profit
from
	products as p
left join
	inventory as i
	on i.product_id=p.product_id
right join
	store as s
	on s.store_id= i.store_id
group by
	p.product_id,
	p.product_name,
	p.product_category,
	s.store_id,
	s.store_location
order by
		profit desc
```



2. The product of product_cost and stock _on_hand was used to get money_tied 

``` SQL

select
	s.store_id,
	p.product_id,
	i.stock_on_hand,
	(p.product_cost * i.stock_on_hand) as money_tied,
	(now()-s.store_open_date) as how_long
from products as p 
left join
	inventory as i
on i.product_id=p.product_id
right join
	store as s
	on s.store_id=i.store_id;
```
3. The CASE expression goes through conditions and returns a value when the first condition is met (like an if-then-else statement). So, once a condition is true, it will stop reading and return the result. If no conditions are true, it returns the value in the ELSE clause.
If there is no ELSE part and no conditions are true, it returns NULL.
``` SQL
select
	p.product_id,
	s.store_location,
	i.stock_on_hand,
	(i.stock_on_hand*p.product_price >0,i.stock_on_hand*p.product_price =0) as profit,
	case
	 when i.stock_on_hand =0 then 'outofstock'
	 else 'available'
	end as stock
from
	products as p
left join 
   inventory as i
on 
	i.product_id=p.product_id
left join
	store as s
	on s.store_id= i.store_id
	;
```
