## Select

```SQL
SELECT select_list [ INTO new_table ]  

[ FROM table_source ] [ WHERE search_condition ]  

[ GROUP BY group_by_expression ]  

[ HAVING search_condition ]  

[ ORDER BY order_expression [ ASC | DESC ] ];
```
SELECT와 FROM사이에 *을 넣어주면 전체 열을 다 볼 수 있으며 특정 열을 보고 싶다면, 

```SQL
SELECT [열]
FROM [테이블]
WHERE [조건];
```
이런 식으로 사용. WHERE 부분은 생략 가능.
***
## Orderby
정렬해줄때 사용, ASC는 오름차순, DESC는 내림차순으로 정렬해주며 지정해주지않으면 오름차순으로 정렬.

```SQL
SELECT select_list FROM table_source
WHERE search_condition
ORDER BY order_expression[ ASC | DESC]; 
```
***
## Select Distinct
Select는 중복여부와 상관없이 모든 자료를 불러오니까 중복되지않은 값만 보고싶을 때 사용.
```SQL
SELECT Distinct select_list FROM table_source;
```
***
## Limit
Limit을 사용하면 위에서부터 지정한 갯수만큼의 자료만을 보여줌.
```SQL
SELECT * FROM 테이블명 LIMIT 숫자;
```
몇개의 제품만을 보고싶다, 판매량이 높은 순위 10개의 제품을 보고 싶다 등 특정 갯수만큼 보고싶을 때 사용함.
***
## Fetch
원하는 행의 갯수만큼의 데이터를 출력하는 옵션, DB 종류에 따라 Limit 혹은 fetch를 사용. (postgreSQL은 둘다 지원)
```SQL
SELECT * FROM 테이블명
FETCH next 5 rows only;
```
이런 형식으로 작성하면 된다. 효율적으로 사용하려면 ORDER BY나 GROUP BY와 같이 사용해야함.
***
## In
WHERE 절 내에서 특정값 여러개를 선택하는 SQL 연산자로 괄호 내의 값 중 일치하는 것이 있으면 TRUE
```SQL
SELECT * FROM 테이블명
WHERE 컬럼명 IN (값1, 값2, ...);
```
***
## Between
BETWEEN A AND B 로 사용하는데, 그대로 해석하면 A와 B사이의 내용을 검색해서 나타내라는 의미

기본 구문은 아래와 같이 사용
```SQL
SELECT * FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

예를 들어 나이가 20이상 30 이하인 사람을 검색하려면
```SQL
SELECT * FROM member
WHERE age BETWEEN 20 AND 30; 
```

Not을 넣어주면 20 이상 30이하가 아닌 사람을 검색할 수 있고, 숫자뿐 아니라 문자도 사용가능.

이런식으로도 표현가능.
```SQL
SELECT * FROM member WHERE age >= 20 AND age <= 30
```
***
## Like
WHERE절에 주로 사용되며 부분적으로 일치하는 칼럼을 찾을 떄 사용.
```SQL
--A로 시작하는 문자를 찾기--
SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE 'A%'

--A로 끝나는 문자 찾기--
SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '%A'

--A를 포함하는 문자 찾기--
SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '%A%'

--A로 시작하는 두글자 문자 찾기--
SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE 'A_'

--첫번째 문자가 'A''가 아닌 모든 문자열 찾기--
SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE'[^A]'

--첫번째 문자가 'A'또는'B'또는'C'인 문자열 찾기--
SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '[ABC]'
SELECT 컬럼명 FROM 테이블 WHERE 컬럼명 LIKE '[A-C]'
```
***
## Isnull
Column이 NULL값일 경우 다른 값으로 대체할 수 있는 기능이 있음.
```SQL
SELECT ISNULL(DEPT,'부서없음') AS DPET
FROM table;
```
부서를 검색하되 부서가 NULL이면 '부서없음'으로 검색하라는 뜻.