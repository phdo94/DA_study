## 서브쿼리
하나의 쿼리 문장 내에 포함된 또 하나의 쿼리 문장, 비교 연산자의 오른쪽에 기술해야 하고 반드시 괄호 안에 넣어야함. 메인 쿼리가 실행되기 이전에 한 번만 실행됨.
- Select절 서브쿼리(스칼라 서브쿼리): Select 절 안에 서브쿼리가 들어있는 구조, 서브쿼리의 결과는 반드시 단일 행이나 SUM, COUNT 등의 집계 함수를 거친 단일 값으로 리턴.<br><br>
예시) 홍길동 학생의 학과를 조회
    ```SQL
    SELECT 학생이름,
        (  SELECT 학과.학과이름
                FROM 학과
            WHERE 학과.학과ID = 학생.학생ID ) AS 학과이름
    FROM 학생
    WHERE 학생이름 = '홍길동' ;
    ```
- From절 서브쿼리(인라인뷰 서브쿼리) : 인라인뷰(Inline view)라고 불리며 From절 안에 서브쿼리가 들어있고 이 때, 서브쿼리의 결과는 반드시 하나의 테이블로 리턴되어야 함.<br><br>
예시) 수학 과목을 수강하는 학생들의 점수 조회
    ```SQL
    SELECT 학생이름, 수학점수
    FROM (SELECT 학생.학생이름 AS 학생이름,
                    과목.과목점수 AS 수학점수
            FROM 학생, 과목
            WHERE 학생.학생이름 = 과목.학생이름
            AND  과목.과목이름 = '수학') ;
    ```
- Where절 서브쿼리(중첩 서브쿼리) : 중첩 서브쿼리(Nested Subqueries)라고 불리며 Where 절안에 서브쿼리가 들어있다. 가장 자주 쓰이는 대중적인 서브쿼리이며 단일행과 복수행 둘 다 리턴이 가능.<br><br>
예시) 수학 과목을 수강하는 학생들의 모든 정보를 조회
    ```SQL
    SELECT *
    FROM 학생
    WHERE 학생.학생이름 IN ( SELECT 과목.학생이름 FROM 과목 WHERE 과목.과목이름 = '수학' ) ;
    ```
- 단일행 서브쿼리 : 서브쿼리의 수행결과가 오직하나의 ROW만을 반환. 이 하나의 결과를 가지고 메인쿼리는 비교 연산자를 통해 쿼리를 수행.
    - 비교 연산자 : 단일행 비교연산자를 사용(>, >=, <, <=, =. ...)
***
## 다중행 서브쿼리
서브쿼리의 수행결과가 두 건 이상의 데이터를 반환.<br>
비교 연산자는 다중행 비교연산자를 사용(IN, ANY, SOME, ALL, EXISTS)
- ANY연산 : 메인쿼리의 비교조건이 서브쿼리의 검색결과와 하나 이상이 일치하면 참.
    - 메인쿼리 < ANY(서브쿼리) : 서브쿼리의 결과와 비교해 메인쿼리의 데이터 중 한개라도 서브쿼리 결과보다 작다면 최소값 반환.
    - 메인쿼리 > ANY(서브쿼리) : 서브쿼리의 결과와 비교해 메인쿼리의 데이터 중 한개라도 서브쿼리 결과보다 크다면 최대값 반환.<br><br>
예시) 30번 소속 사원들 중 급여를 가장 많이 받는 사원보다 더 많은 급여를 받는 사람의 이름과 급여를 출력
    ```SQL
    SELECT  ENAME, SAL
    FROM  EMP
    WHERE  SAL > ALL ( SELECT  SAL
                        FROM  EMP
                        WHERE  DEPTNO = 30 );
    ```
- ALL연산 : 메인쿼리의 비교조건이 서브쿼리의 검색결과와 모든 값이 일치하면 참.
    - 메인쿼리 < ALL(서브쿼리) : 서브쿼리의 결과와 비교하여 최소값 반환.
    - 메인쿼리 > ALL(서브쿼리) : 서브쿼리의 결과와 비교하여 최대값 반환.

- EXISTS연산 : 메인쿼리의 비교조건이 서브쿼리의 검색결과중에 하나라도 만족하는 값이 존재하면 참. 해당 row가 존재하는 지 여부만 확인, Not exists는 메인쿼리의 컬럼명과 서브쿼리의 컬럼명을 비교하여 일치하지 않으면 메인쿼리 테이블의 모든 ROW(행)을 반환.
***
## 조인과 집계데이터 
- Grouping Set절<br>
오라클에서 소계, 합계, 총계의 쿼리(SQL)를 작성할 때는 ROLLUP을 많이 사용. ROLLUP의 경우 나열된 컬럼의 단계별로 소계, 합계를 자동으로 집계. 그에 반해 GROUPING SETS는 여러 그룹핑 쿼리를 UNION ALL 한 것과 같은 결과를 만들 수 있어 조금 더 유연하게 소계, 합계를 집계할 수 있음.
```SQL
SELECT job ,deptno, COUNT(*) cnt 
FROM emp 
GROUP BY GROUPING SETS((job, mgr), (job, deptno), ())

#이런식으로 사용
GROUPING SETS( 컬럼, 컬럼, 컬럼, ... )
GROUPING SETS( (컬럼그룹), (컬럼그룹), (컬럼그룹), ... )
```
- Roll up 절<br>
Group by를 통해 그루핑을 진행한다음 최종집계하기위해 사용하는 함수.<br>
예를 들어 group by를 통해 성별로 묶는다면, 남자 총 몆명, 여자 총 몇명을 알려주지만, rollup은 한번 더 나아가서 이 둘을 더해서 총 몇명까지 보여줌.
    ```SQL
    Select [조회하고자 하는 부분]
    from [테이블]
    where [테이블에서 group by 진행 전 필터링 조건]
    group by Rollup (묶는 기준)
    having [group by 결과 필터링할 조건]
    ```
- Cube 절
<br> cube는 rollup보다 조금 더 상세한 결과를 내줌. 그리고 그룹을 묶어주는 방식이 다름.
    ```SQL
    Select [조회하고자 하는 부분]
    from [테이블]
    Group by rollup (a, b, c) : (a, b, c) / (a, b) / (a) / ()
    Group by cube (a, b, c) : (a, b, c) / (a, b) / (a, c) / (b, c) / (a) / (b) / (c) / ()
    ``` 

- 분석 함수란
<br> 테이블에 있는 열에 대해 특정 그룹병로 집계값을 산출할 때 사용. Group by가 그룹별로 로우 수가 줄어들게 하는데 반해, 집계함수는 열에 대한 정보 손실이 없이 집계값을 산출 가능. 분석함수에서 사용하는 열 별 그룹을 Window하고 함. 이는 열의 범위를 결정하는 역할을 함.
    ```SQL
    Select [조회하고자 하는 부분]
    from [테이블]
    분석함수(매개변수) Over (Partition by[로우그룹을 지정] EXPR1, EXPR2, ..., order by[파티션 내부 순서를 지정] EXPR3, EXPR4, ..., Window[더 상세한 그룹으로 분할할 때 사용] ...)
    ```

- AVG함수
<br>: 말 그대로 평균을 내주는 함수.
    ```SQL
    -- 분석함수
    SELECT [컬럼이름], AVG(목표 컬럼) Over()
    FROM [테이블명]
    ```

- Row Number , Rank , Dense_Rank 함수
<br>: 그룹내 순위를 결정해주는 함수.
    ```SQL
    select ROW_NUMBER() over(partition by [그룹핑할 컬럼] order by [정렬할 컬럼])

    RANK() over(partition by [그룹핑할 컬럼] order by [정렬할 컬럼])

    DENSE_RANK() over(partition by [그룹핑할 컬럼] order by [정렬할 컬럼])
    from 테이블명
    ```
- First_Value , Last_Value함수
    - first_value : 그룹 별 데이터를 정렬한 후 지정한 변수의 첫번째 값을 반환. 집합 내에서 첫 번째 값이 null이고 따로 조건을 지정해주지않으면 null을 반환.<br>
    - last_value : 그룹 별로 데이터를 정렬한 후 지정한 변수의 마지막 값을 반환.
    ```SQL
    Select * from [테이블명]
    Last_value | First_value ( [ scalar_expression ] )  [ IGNORE NULLS | RESPECT NULLS ]
    OVER ( [ partition_by_clause ] order_by_clause rows_range_clause)
    ```      
- Lag, Lead 함수
<br> : lag와 lead 함수는 계산 대상 데이터들을 order by 절로 정렬하여 expr에 명시된 값을 기준으로 이전(lag)이나 이후(lead)의 값을 반환.(offset, default 모두 생략 가능)
    ```SQL
    SELECT
        SEQ
        ,TITLE
        ,CONTENT
        ,LAG(SEQ,1,0) OVER (ORDER BY SEQ) AS PRE_SEQ
        ,LAG(TITLE,1,'이전글없음') OVER (ORDER BY SEQ) AS PRE_TITLE
        ,LEAD(SEQ,1,0) OVER (ORDER BY SEQ) AS NEXT_SEQ
        ,LEAD(TITLE,1,'이후글없음') OVER (ORDER BY SEQ) AS NEXT_TITLE
    FROM NOTICES
    ORDER BY SEQ;
    ``` 
***
## 조건 연산자 , with 문 , 트랜잭션 
- 조건연산자 : 산술, 비교, 논리 연산자 순으로 우선 순위가 정해짐.
    - 산술 연산자: Select로 칼럼을 선택할 때, 칼럼의 값에 +, -, *, /, % 산술연산자를 이용한 값을 반환할 수 있음.<br><br>
        ```SQL
        Select column1, column1 + 10 ...
        From table_name
        ```
    - 비교 연산자 : 레코드를 검색할 때, 특정 조건과 값을 비교하여 부합하는 레코드만 검색 가능. =, <, >, <=, >+, <>등을 이용해서 조건과 같거나 같지않음, 크거나 작음 등을 비교.
        ```SQL
        SELECT column_names
        FROM table_name
        WHERE column_name >= 10;
        ```
        - In 연산자는 Where 조건문의 값을 여러 개로 정할 수 있음. or보다 간단한 조건문을 만들 때 사용.
            ```SQL
            SELECT column_name(s)
            FROM table_name
            WHERE column_name IN (value1, value2, ...);
            ```
    - 논리 연산자 : Where 문은 And, Or, Not 연산자와 결합될 수 있음. And와 Or 연산자는 하나 이상의 조건을 이용해 레코드를 필터링할 때 주로 사용.
        - And : 모든 조건을 만족한 레코드를 선택.
        - Or : 조건을 하나라도 만족한 레코드를 선택.
        - Not : 조건 값이 아닌 레코드를 선택.
            ```SQL
            SELECT column1, column2, ...
            FROM table_name
            WHERE condition1 AND condition2 AND condition3 ...;
            ```
- With절 : 이름이 부여된 서브쿼리라고 생각하면 됨. 임시 테이블을 만든다는 관점에서 본다면 view와 쓰임새가 비슷한데 차이점이 있다면 view는 한번 만들어놓으면 drop할때 까지 없어지지 않지만 with절의 경우 한번 실행할 쿼리문내에 정의되어 있을 경우, 쿼리문 안에사만 실행된다는 차이점이 있음.
    - with 절 사용방법(간단하게)
        ```SQL
        WITH EXAMPLE AS
        (SELECT 'WITH절' AS STR1
        FROM DUAL);
        
        SELECT * FROM EXAMPLE
        ```
        with절은 한번이 아니라 여러번 사용해야할 때 적절함.
- 트랜잭션(transaction) : '거래'라는 뜻으로 데이터베이스 내에서 하나의 그룹으로 처리되어야 하는 명령문들을 모아 놓은 논리적인 작업 단위이다. 여러 단계의 처리를 하나의 처리처럼 다룰 수 있고 여러 개의 명령어 집합이 정상적으로 처리되면 정상 종료. 하나의 명령어라도 잘못되면 전체취소. 트랜잭션을 쓰는 이유는 데이터의 일관성을 유지하면서 안정적으로 데이터를 복구하기 위함.
    <img src='https://t1.daumcdn.net/cfile/tistory/9978DA4F5ADE84AD15'>
    - Commit : 완전 저장 기능. 모든 작업들을 정상적으로 처리하겠다고 확정하는 명령어로 처리과정을 DB에 영구 저장하는 것. commit을 수행하면 하나의 트랜잭션 과정을 종료하는 것.
    - Rollback : 작업 중 문제가 발생되어 트랜잭션의 처리과정에서 발생한 변경사항을 취소하는 명령어. 마지막 commit을 완료한 시점으로 다시 돌아감.
    - Savepoint : 임시저장 기능, 보통 rollback을 하면 작업전체가 취소되는데 전체가 아닌 특정 부분에서 트랜잭션을 취소 시킬 수 있음. savepoint를 쓰면 현재의 트랜잭션을 작게 분할 가능.
    ```SQL
    CREATE TABLE tran(
        id number NOT NULL Primary key,
        first_name varchar2(20),
        last_name varchar2(20),
        age varchar2(10),
        birth date
        );
        
        INSERT INTO trat3 values(2012,'황','요셉','26','92.06.18');
        INSERT INTO trat3 values(2014,'심','준식','24','95.01.12');
        INSERT INTO trat3 values(2015,'서','예슬','23','95.05.22');
        INSERT INTO trat3 values(2011,'김','도훈','28','91.09.26');
        SELECT * FROM TRAT3;
 
        COMMIT;
    ```
    - savepoint 예시
        ```SQL
        DELETE FROM TRAT3 WHERE ID=2012;
        COMMIT;
        SAVEPOINT S1;
        DELETE FROM TRAT3 WHERE ID=2014;
        SAVEPOINT S2;
        DELETE FROM TRAT3 WHERE ID=2015;
        SAVEPOINT S3;
        DELETE FROM TRAT3 WHERE ID=2011;
        ROLLBACK TO S2;
        ```
        - 마지막에 rollback to S2를 선언했으므로. S2로 돌아감.
        - commit을 나중에 해주면 앞에 있는 savepoint는 날라감.
***
## window function 
***
## CTE
***
## Partition by