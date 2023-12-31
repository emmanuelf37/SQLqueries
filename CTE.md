# Here is a query that shows the top 10 customers for Rockbuster in terms of customer numbers:

```sql

SELECT D.country,
       COUNT(customer_id) AS count_of_customers
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_ID = D.country_ID
GROUP BY country
ORDER BY count_of_customers DESC
LIMIT 10;

```

# Here is a query finding the top 5 customers in the top 10 cities who have paid the highest total amounts to Rockbuster

```sql

SELECT A.customer_id, B.first_name, B.last_name, E.country, D.city,
Sum(A.amount) as Total_Payment_Amount
FROM payment A
INNER JOIN customer B ON A.customer_id = B.customer_id
INNER JOIN address C ON B.address_id = C.address_id
INNER JOIN city D ON C.city_id = D.city_id
INNER JOIN country E ON D.country_ID = E.country_ID
WHERE D.city in ('Aurora','Tokat','Tarsus', 'Atlixco', 'Emeishan', 'Pontianak', 'Shimoga', 'Aparecida de Goinia', 'Zalantun', 'Taguig')

GROUP BY A.customer_id, B.first_name, B.last_name, E.country, D.city
ORDER BY total_payment_amount DESC
LIMIT 5;

```

# Here is a query finding the average amount paid by these top 5 customers

```sql

Select avg(total_payment_amount) as "average"
from
(select a.customer_id, b.first_name, b.last_name, e.country, d.city,
sum(a.amount) as total_payment_amount
from payment A
inner join customer B on a.customer_id = b.customer_id
inner join address c on b.address_id = c.address_id
inner join city d on c.city_id = d.city_id
inner join country e on d.country_id = e.country_id
where d.city in ('Aurora','Tokat','Tarsus', 'Atlixco', 'Emeishan', 'Pontianak', 'Shimoga', 'Aparecida de Goinia', 'Zalantun', 'Taguig')

group by a.customer_id, b.first_name, b.last_name, e.country, d.city
order by total_payment_amount DESC
limit 5) as total_payment_amount

```

# Here is a query finding out how many of the top 5 customers are based in each country

```sql

SELECT D.country,

count(A.customer_id) as "All Customer Count",
count(top_customer_count) as "top_customer_count"
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C on B.city_id = c.city_id
INNER JOIN country D on C.country_id = D.country_id
left join (SELECT A.customer_id, A.first_name, A.last_name, D.country, C.city,
Sum(E.amount) as Total_Payment_Amount
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C on B.city_id = C.city_id
INNER JOIN country D on C.country_id = D.country_id
INNER JOIN payment E ON A.customer_id = E.customer_id
WHERE c.city in ('Aurora','Tokat','Tarsus', 'Atlixco', 'Emeishan', 'Pontianak', 'Shimoga', 'Aparecida de Goinia', 'Zalantun', 'Taguig')

GROUP BY A.customer_id, A.first_name, A.last_name, D.country, C.city
ORDER BY Total_Payment_Amount DESC
LIMIT 5) as top_customer_count on a.customer_id = top_customer_count.customer_id
group by D.Country
Having count (top_customer_count) >0
order by count(top_customer_count), Count(A.customer_id) DESC

```
