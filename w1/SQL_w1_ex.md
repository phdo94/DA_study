문제1번) dvd 렌탈 업체의  dvd 대여가 있었던 날짜를 확인해주세요.

```SQL
SELECT Distinct date(rental_date)
FROM rental;
```

문제2번) 영화길이가 120분 이상이면서, 대여기간이 4일 이상이 가능한, 영화제목을 알려주세요.
```SQL
SELECT title from film
WHERE length >= 120 AND rental_duration >= 4; 
```
문제3번) 직원의 id 가 2 번인  직원의  id, 이름, 성을 알려주세요
```SQL
SELECT staff_id, first_name, last_name FROM staff
WHERE staff_id = 2;
```
문제4번) 지불 내역 중에서,   지불 내역 번호가 17510 에 해당하는  ,  고객의 지출 내역 (amount ) 는 얼마인가요?
```SQL
SELECT amount FROM payment 
WHERE payment_id = 17510;
```
문제5번) 영화 카테고리 중에서 ,Sci-Fi  카테고리의  카테고리 번호는 몇번인가요?
```SQL
SELECT category_id FROM category
WHERE name = 'Sci-Fi';
```
문제6번) film 테이블을 활용하여, rating  등급(?) 에 대해서, 몇개의 등급이 있는지 확인해보세요.
```SQL
SELECT distinct rating FROM film;
```
문제7번) 대여 기간이 (회수 - 대여일) 10일 이상이였던 rental 테이블에 대한 모든 정보를 알려주세요.
단 , 대여기간은  대여일자부터 대여기간으로 포함하여 계산합니다.
```SQL
select *, date(return_date) - date(rental_date) +1 
	as diff_date 
from rental
where date(return_date) - date(rental_date) +1 >= 10
```

***
문제 7번의 경우 column끼리의 연산을 어떻게 사용하는 지 알아야 풀 수 있으며, Where절이 Select절보다 먼저 실행되기 때문에 diff_date를 활용하지 못한다는 것 또한 알고 있어야 해결이 가능하다.
***
문제8번) 고객의 id 가  50,100,150 ..등 50번의 배수에 해당하는 고객들에 대해서,
회원 가입 감사 이벤트를 진행하려고합니다.
고객 아이디가 50번 배수인 아이디와, 고객의 이름 (성, 이름)과 이메일에 대해서
확인해주세요.
```SQL
select customer_id, first_name, last_name, email 
from customer
where customer_id % 50 = 0
```
문제9번) 영화 제목의 길이가 8글자인, 영화 제목 리스트를 나열해주세요.
```SQL
select length(title), title from film
where length(title) =8
order by title;
```
문제10번)	city 테이블의 city 갯수는 몇개인가요?
```SQL
select distinct count(city) from city
```
문제11번)	영화배우의 이름 (이름+' '+성) 에 대해서,  대문자로 이름을 보여주세요.  단 고객의 이름이 동일한 사람이 있다면,  중복 제거하고, 알려주세요.
```SQL
select distinct upper(first_name ||' '|| last_name)
from actor
```
***
문제 11번은 숫자가 아닌 문자열로 구성되어 있는 컬럼을 어떤 방법으로 합치는 지,
전부 대문자로 바꾸는 방법은 무엇인지 알고 있어야 해결이 가능한 문제이다.
***
문제12번)	고객 중에서,  active 상태가 0 인 즉 현재 사용하지 않고 있는 고객의 수를 알려주세요.
```SQL
select count(active) from customer
where active = 0
```
문제13번)	Customer 테이블을 활용하여,  store_id = 1 에 매핑된  고객의 수는 몇명인지 확인해보세요.
```SQL
select count(distinct customer_id) from customer as c
where store_id = 1
```
문제14번)	rental 테이블을 활용하여,  고객이 return 했던 날짜가 2005년6월20일에 해당했던 rental 의 갯수가 몇개였는지 확인해보세요.
```SQL
select count(distinct rental_id) from rental
where date(return_date) ='2005-06-20';
```
문제15번)	film 테이블을 활용하여, 2006년에 출시가 되고 rating 이 'G' 등급에 해당하며, 대여기간이 3일에 해당하는  것에 대한 film 테이블의 모든 컬럼을 알려주세요.
```SQL
select * from film
where release_year = 2006 and rating = 'G' and rental_duration = 3
```
문제16번)	langugage 테이블에 있는 id, name 컬럼을 확인해보세요 .
```SQL
select language_id, name from "language"
```
문제17번)	film 테이블을 활용하여,  rental_duration 이  7일 이상 대여가 가능한  film 에 대해서  film_id,   title,  description 컬럼을 확인해보세요.
```SQL
select film_id, title, description from film
where rental_duration >= 7
```
문제18번)	film 테이블을 활용하여,  rental_duration   대여가 가능한 일자가 3일 또는 5일에 해당하는  film_id,  title, desciption 을 확인해주세요.
```SQL
select film_id, title, description from film
where rental_duration =3 or rental_duration =5
```
문제19번)	Actor 테이블을 이용하여,  이름이 Nick 이거나  성이 Hunt 인  배우의  id 와  이름, 성을 확인해주세요.
```SQL
select actor_id, first_name, last_name from actor 
where first_name = 'Nick' or last_name = 'Hunt'
```
문제20번)	Actor 테이블을 이용하여, Actor 테이블의  first_name 컬럼과 last_name 컬럼을 , firstname, lastname 으로 컬럼명을 바꿔서 보여주세요
```SQL
select first_name as firstname, last_name as lastname from actor
```
수정