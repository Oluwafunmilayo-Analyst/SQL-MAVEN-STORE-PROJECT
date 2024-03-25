# MAVEN-STORE-PROJECT WITH ANALYSIS SQL
Sales and inventory data analysis for a fictitious share of toy store in Mexico.
. 
1. Which product category drive the biggest profits? is it the same across the store locations?
2. How much money is tied up in inventory at the toy stores? How long will it last?
 3. Are sales being lost with out-of-stock products at certain location? 
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
	;
```
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

``` SQL
select
	p.product_id,
	s.store_location,
	i.stock_on_hand,
	(i.stock_on_hand*p.product_price >0 -i.stock_on_hand*p.product_price =0) as profit,
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
