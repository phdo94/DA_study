## JOIN
둘 이상의 테이블을 연결해서 데이터를 검색하는 방법, join의 종류에는 Inner join, Outer join, Self join, Full outer join, Cross join, Natural join이 있다.<br><br>
둘 이상의 테이블이 연결되려면 적어도 하나의 컬럼을 공유하고 있어야함. 이때 공유하고 있는 컬럼을 PK 또는 FK로 사용
<br><br>
- 명시적 표현의 경우
    ```SQL
    Select * from employee Inner Join department
    On employee.DepartmentID = department.departmentID;
    ```
    테이블에 조인을 하라는 것을 지정하기 위해 'JOIN' 키워드를 사용하고 ON 키워드를 조인에 대한 구문을 지정하는데 사용.
<br><br>
- 암시적 표현의 경우
    ```SQL
    Select * from employee, department
    Where employee.DepartmentID = department.DepartmentID;
    ```
    암시적 조인 표현은 Select 구문의 from 절에 ','를 사용하여 단순히 조인을 위한 여러 테이블을 나열하기만 하면 됨.
<br><br>

- Inner join : 내부 조인 -> 교집합, 키 값이 있는 테이블의 컬럼 값을 비교 후 조건에 맞는 값을 가져오는 것, 쉽게 말해서 서로 연관된 내용만 검색하는 조인 방법.<br>
    <img src="https://t1.daumcdn.net/cfile/tistory/251A374456EB994D13">

- Outer join : 조인하는 여러 테이블에서 한 쪽에는 데이터가 있고 한쪽에는 데이터가 없는 경우, 데이터가 있는 쪽 테이블의 내용을 전부 출력하는 방법. 즉, 조인 조건에 만족하지 않아도 해당 행을 출력하고 싶을 때 사용<br>
    - Left Outer Join : 조인문의 왼쪽에 있는 테이블의 모든 결과를 가져온 후 오른쪽 테이블의 데이터를 매칭하고, 매칭되는 데이터가 없는 경우 NULL을 표시.<br><img src="https://t1.daumcdn.net/cfile/tistory/224EFA4656EF49B309"><br>
    - Right Outer Join : Left Outer Join과 반대로 생각하면 쉽다. 조인문 오른쪽에 있는 테이블의 모든 결과를 가져온 후 왼쪽의 데이터를 매칭하고, 매칭되는 데이터가 없는 경우 NULL을 표시.<br>  
    <img src="https://t1.daumcdn.net/cfile/tistory/2418A25056EF4BA912"><br>
- Self join : 동일 테이블 사이의 조인을 말함. 따라서 From절에 동일 테이블이 두번 이상 나타남.
    <br><br> 
    - 기본 구조
    ```SQL
    SELECT 별명1.컬럼, 별명2.컬럼, …
    FROM 테이블 별명1, 테이블 별명2, …
    WHERE 조인 조건
    [AND 일반 조건]
    ``` 
    <br> 동일 테이블에서 동일한 컬럼을 비교해야할 때 사용한다. 쉽게 이해할 수 있는 예시는 좀 더 공부 해야할 듯...

- Full outer join : Outer Join의 종류 중 하나로 Left Outer Join과 Right Outer Join을 합친 것. 양쪽 모두 조건이 일치하지 않는 것들까지 모두 결합하여 출력<br>
    <img src="https://t1.daumcdn.net/cfile/tistory/232EF54356EF4DA123"><br>

- Cross join : 두 개의 테이블을 아무 조건없이 연결시키는 조인을 말함. 조인 시켰을 때 컬럼의 수가 얼마나 나올 지는 카테션 곱으로 구할 수 있다. 일반적인 조인은 조건이 참인 것만 연결하지만 Cross join은 데이터를 무조건 연결함.
    
- Natural join : Natural join은 다른 테이블에 동일한 타입과 이름을 가진 컬럼이 있다면 조인 조건으로 조인을 간단히 표현하는 방법.<br>
    - 기본 구조
    ```SQL
    SELECT 컬럼, 컬럼, …
    FROM 테이블1
    NATURAL JOIN 테이블2
    [NATURAL JOIN 테이블3] …
    WHERE 검색 조건;
    ```
***
## Group by
Group by는 검색된 여러 행을 이용하여 통계정보를 계산하는 함수.
```SQL
SELECT [DISTINCT] 컬럼, 그룹 함수(컬럼)
FROM 테이블명
[WHERE 조건]
[GROUP BY Group대상]
[ORDER BY 정렬대상 [ASC/DESC]]
```
***
## Having
Having절은 Where절과 동일하다고 생각하면 쉽다. 단 조건 내용에 그룹함수를 포함하는 것만 작성한다. 일반 조건을 Where절에 기술하고 그룹 함수를 포함한 조건은 Having절에 기술한다.
```SQL
SELECT [DISTINCT] 컬럼, 그룹 함수(컬럼)
FROM 테이블명
[WHERE 조건]
[GROUP BY Group대상]
[HAVING 그룹 함수 포함 조건]
[ORDER BY 정렬대상 [ASC/DESC]]
```
***
## 집합연산자와 서브쿼리
- Union연산 : 합집합을 의미, 중복을 제거한 결과의 합을 검색 

- UnionAll연산 : 합집합을 의미, 중복을 포함한 결과의 합을 검색

- intersect연산 : 교집합을 의미, 양쪽 모두에서 포함된 행을 검색

- Except연산 : 차집합을 의미, 첫 번째 검색 결과에서 두 번째 검색 결과를 제외한 나머지를 검색 (일부 데이터베이스에서는 MINUS를 사용한다.)
<br><br> 기본구조
```SQL
SELECT 컬럼명, 컬럼명2 ...
FROM 테이블명1
[UNION | UNION ALL | INTERSECT | MINUS]
SELECT 컬럼명, 컬럼명2 ...
FROM 테이블명2
[ORDER BY 컬럼 [ASC/DESC]];
```
- 두 Select절의 컬럼 개수와 데이터 타입은 일치해야한다.
- 검색 결과의 헤터는 앞쪽 Select문에 의해 결정된다.
- Order by절을 사용할때는 문장의 제일 마지막에 사용한다.