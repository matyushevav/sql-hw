# Домашнее задание к занятию "`Расширенные возможности SQL`" - `Матюшев Анатолий Владимирович`

```sql
-- Задание 1
-- Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:
-- фамилия и имя сотрудника из этого магазина;
-- город нахождения магазина;
-- количество пользователей, закреплённых в этом магазине.
select b.last_name, b.first_name, d.city, e.customer_count from sakila.store a
join sakila.staff b on a.manager_staff_id = b.staff_id 
join sakila.address c on a.address_id = c.address_id 
join sakila.city d on c.city_id = d.city_id 
join 
	(select store_id, count(distinct customer_id) as customer_count from sakila.customer
	group by store_id 
	having count(distinct customer_id) > 300) e 
on a.store_id = e.store_id

-- Задание 2
-- Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
select count(*) from sakila.film
where `length` > (select avg(length) from sakila.film)


-- Задание 3
-- Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
select a.month, a.total_payment_sum, b.total_rental_count from
	(select monthname(payment_date) as month, sum(amount) as total_payment_sum from sakila.payment
	group by monthname(payment_date)
	order by sum(amount) desc 
	limit 1) a
join 
	(select monthname(rental_date) as month, count(rental_id) as total_rental_count from sakila.rental
	group by monthname(rental_date)) b
on a.month = b.month
```

Скриншот к заданию 1:
![Скриншот-1](https://github.com/matyushevav/sql-hw/blob/main/img/1.png)

Скриншот к заданию 2:
![Скриншот-1](https://github.com/matyushevav/sql-hw/blob/main/img/2.png)

Скриншот к заданию 4:
![Скриншот-1](https://github.com/matyushevav/sql-hw/blob/main/img/3.png)