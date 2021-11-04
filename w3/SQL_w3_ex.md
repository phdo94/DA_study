문제1번) 매출을 가장 많이 올린 dvd 고객 이름은? (subquery 활용)
```SQL
select first_name, last_name from customer as c
where customer_id in (
	select customer_id
	from payment as p
	group by customer_id
	order by sum(amount) desc
	limit 1
	) 
```
문제2번) 대여가 한번도이라도 된 영화 카테 고리 이름을 알려주세요. (쿼리는, Exists조건을 이용하여 풀어봅시다)
```SQL
select c.name from category as c
where exists (
	select fc.category_id
	from rental as r
	join inventory as i on r.inventory_id = i.inventory_id
	join film_category as fc on i.film_id = fc.film_id
	where fc.category_id = c.category_id
	) 
```
문제3번) 대여가 한번도이라도 된 영화 카테 고리 이름을 알려주세요. (쿼리는, Any 조건을 이용하여 풀어봅시다)
```SQL
select c.name from category as c
where category_id = any(
	select fc.category_id
	from rental as r
	join inventory as i on r.inventory_id = i.inventory_id
	join film_category as fc on i.film_id = fc.film_id
	) 
```
문제4번) 대여가 가장 많이 진행된 카테고리는 무엇인가요? (Any, All 조건 중 하나를 사용하여 풀어봅시다)
```SQL
select c.*
from category as c
where category_id = all(
	select fc.category_id
	from rental as r
	join inventory as i on r.inventory_id = i.inventory_id
	join film_category as fc on i.film_id  = fc.film_id
	group by fc.category_id
	order by count(distinct r.rental_id) desc
	limit 1)
```
- select * 해보면서 테이블을 관찰해보면 categoty만을 사용해서는 도저히 답이 안나온다 라는 것을 깨닫게 됨.
- category_id를 공유하고 있으며, rental이 얼마나 많이 됐는지 측정할 수 있는 테이블을 활용해야함.
- 일단 rental 테이블로 어떤 카테고리를 빌려갔는지 알 수 없어서,
카테고리를 연결시켜주기위해 film_category, inventory 테이블 활용.
- 모든 테이블 join시켜주고 그루핑 시켜서 대여량이 가장 많은 category_id 추출
- category 테이블과 all로 비교(조건이 한개만 있으니 any, all 둘 중에 하나만 써도 ok)<br>

문제5번) dvd 대여를 제일 많이한 고객 이름은? (subquery 활용)
```SQL
select c.first_name, c.last_name from customer as c
where c.customer_id = any(
	select p.customer_id
	from payment as p
	group by p,customer_id
	order by count(p.rental_id) desc
	limit 1)
```
문제6번) 영화 카테고리값이 존재하지 않는 영화가 있나요?
