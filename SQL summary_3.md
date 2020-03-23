# 불친절한 SQL 프로그래밍
## 중급 쿼리
### 조인
조인은 열값으로 테이블 행을 연결한다. 하나의 행은 하나 이사의 행과 연결될 수 있다. 부모테블과 자식 테이블은 기본키와 외래 키로 조인하는 것이 일반적이지만, 관계르 가지지 않는 테이블을 조인하는 경우도 있다.
#### 기본원리
##### 조인 조건
조인조건은 조인할 두 테이블의 열이 기술된 조건이다. 

    CREATE TABLE t1 NUMBER, CONSTRAINT t1_pk PRIMARY KEY (c1);
    CREATE TABLE t2 NUMBER, CONSTRAINT t2_pk PRIMARY KEY (c1);
    CREATE TABLE t3 NUMBER, CONSTRAINT t3_pk PRIMARY KEY (c1);
    
    INSERT INTO t1 VALUES (1);
    INSERT INTO t1 VALUES (2);
    INSERT INTO t1 VALUES (3);
    INSERT INTO t2 VALUES (1);
    INSERT INTO t2 VALUES (2);
    INSERT INTO t3 VALUES (1);
    INSERT INTO t3 VALUES (3);
    COMMIT;
###### 카티션 곱
카티션곱은 조인조건이 없는 조인이다. 의도한 경우가 아니라면 조인조건이 누락되었을때 발생한다.
###### 등가 조인 
등가조인은 조인조건이 모두 등호인 조인이다. 값이 동일한 경우에만 행이 반환된다.
###### 비등가 조인
비등가조인은 등호외의 다른조인 조건이 있는 조인이다.
##### 조인범위
조인범위는 이너와 아우터로 구분할 수있다. 조인이 성공한 범위를 이너, 실패한 범위를 아우터 이다.,
###### 이너조인
이너조인은 조인이 성공한 범위를 반환한다. 
###### 아우터 조인 
아우터조인은 이너와 아우터를 함께 반환한다. 
#### 조인 치수
조인치수는 테이블의 차수를 의미한다. 데이터 모델의 관계차수와 유사한 개념이지만 조인조건, 조인기준에 따라 변경될 수 있다는점이 다르다
#### 기술순서
##### FROM절
FROM절의 테이블은 데이터의 논리적인 흐름에따라 기술해야한다. 데이터의 논리적인 흐름은 아래의 규칙을 통해 결정할 수있다."데이터 모델에 따라 조인순서를 결정하고, 업무 요건에 따라 조인순서를 조정한다."
##### WHERE절
 WHERE절도 데이터의 논리적인 흐름에따라 작성해야한다. 
 "FROM절에 첫 번째로 기술된 테이블의 일반 조건을 기술한다음, FROM절에 기술한 테이블의 순서에 따라 조인조건과 일반 조건의 순서로 조건을 기술한다. 조인 조건은 가능한 PK와 FK 순서대로 기술하고,먼저조회돤 테이블이 값이 입력되는 형태로 작성된다."
#### ANSI 조인 문법
##### NATURAL JOIN절
 NATURAL JOIN절은 이름이 같은 열로 테이블을 등가  교환한다.아래 쿼리는 이름이 같은 열 로 T1과 T2테이블을 등가교환한다.
 

    SELECT * FROM T1 NATURAL JOIN T2
##### USING절
USING 절은 지정한 열로 테이블을 증가 조인한다. 지정한열은 조인할 테이블에 동일한 이름으로 존재해야한다.아래의 쿼리는 t1,t2테이블을 등가조인한다.

    SELECT * FROM t1 JOIN t2 USING (c1);
##### CROSS JOIN 절
CROSS JOIN절은 카티션 곱을 생성한다.아래의 커리는 CROSS JOIN절을 사용한 쿼리다. 

    SELECT a.c1 AS a,b.c1 AS b
	    FROM t1 a
	CROSS
		JOIN t2 b;
	=========================================	
	SELECT a.c1 AS a,b.c1 AS b
		FROM t1 a
			, t2 b;
		
		
### 서브쿼리
쿼리블럭은 다른 쿼리 블록을 포함 할수있다. 다른 쿼리 블러에 포함된 쿼리 블럭을 서브 쿼리 , 다른 쿼리 블럭을 메인 뭐리 라고 한다
![서브 쿼리 이미지 검색결과](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcR3-Njf5FnO1saV6OxebnHT6zz8i8j6Z-O6KoluoOR5x2dVQhE8)
서브 쿼리는 사용 위치에 따라 중첩서브쿼리 ,스칼라 서브 쿼리 , 인라인 뷰로 구분된다.
|서브 쿼리|사용 위치|
|--|--|
|중첩 서브 쿼리|WHERE 절, HAVING 절|
|스칼라 서브 쿼리|SELECT 절|
|인라인 뷰|FROM 절|
#### 중첩 서브쿼리
중첩서브쿼리는 WHERE절과 HAVING절에 사용되는 서브 쿼리이다.
|서브 쿼리|설명|
|--|--|
|비상관 서브 쿼리|메인쿼리와 관계 없음|
|상관 서브 쿼리|메인쿼리와 관계 있음|
|단일 서브 쿼리|서브 쿼리가 딘일 행을 반환|
|다중 서브 쿼리|서브 쿼리가 다중 행을 반환|
##### 비상관 서브 쿼리
비상관 서브쿼리는 메인 쿼리와 관계가 없는 서브 쿼리이다. 서브쿼리으 WHERE 절에 메인 쿼리와의 조인조건이 존재하지 않는다.
##### 단일 행 비상관 서브쿼리
주로  일떄 비교조건을 사용한다.

    SELECT * FROM T1 WHERE C1= (SELECT MAX(C1)AS C1 FROM T2);

##### 다중행 비상관 서브쿼리
주로 IN조건을 사용한다
###### IN조건
IN조건은 서브쿼리의 결과에 해당하는 메인쿼리의 행을 조회한다.

    SELECT * FROM WHERE C1 IN (SELECT C1 FROM T2);
###### NOT IN조건
NOT IN조건은 서브쿼리의 결과에 해당하지 않는 메인쿼리의 행을 조회한다.

    SELECT * FROM WHERE C1 NOT IN (SELECT C1 FROM T2);

###### ANY 조건과 ALL조건
ANY조건은 서브쿼리 결과의 일부, ALL 조건은 서브 쿼리의 전체를 비교하여 조건에 만족하는 행을 반환한다.

|조건|ANY조건|ALL조건|
|--|--|--|
|=|IN(subquery)||
|<>||NOT IN(subquery)|
|>|> (SELECT MIN ... subquery)|> (SELECT MAX ... subquery)|
|<|< (SELECT MAX ... subquery)|< (SELECT MIN ... subquery)|
##### 상관 서브 쿼리
서브쿼리의 WHERE절에 메인 쿼리와의 조인 조건이 존재한다. 
###### 단일 행 상관 서브쿼리
주로 비겨 조건을 사용한다.
아래의 쿼리는 메인쿼리의 열을 조인 하여 반환하는 결과를 메인 쿼리의 열과 비교한다.

    SELECT a.* FROM t1 a WHERE a.c2 = (SELECT MAX (X.C2) FROM t2 x WHERE x.c1 = a.c1);
##### 다중 행 상관 서브 쿼리 
다중 행 상관 서브 커리는 다중행을 반환하는 상관 서브 쿼리다. 주로 EXISTS 조건을 사용한다.

    [NOT]EXISTS(subquery)

###### EXSTS 조건
EXSTS 조건은 메인 쿼리의 행이 서브쿼리에 존재하는지 확인한다. 서브쿼리에 존재하는 행을 반환한다.

    SLELCT a.* FROM t1 WHERE EXISTS (SELECT 1 FROM t2 X WHERE x.c1 =a.c1)
위 쿼리는 아래릐 슈도 코드로 표현할 수 있다. EXISTS 조건은 서브 쿼리와의 조인이 1번이라도 성공하면 행을 반환한다.이런 조인 방식을 세미 조인이라고 한다.

    FOR a IN (SELECT * FROM t1)LOOP
	    FOR b IN (SELECT * FROM t2 b WHERE b.c1 = a.c1)LOOP
		    EXIT;
		END LOOP;
		결과 반환
	END LOOP;
###### NOT EXISTS 조건 
NOT EXISTS 조건은 메인 쿼리릐 행이 서브쿼리에 존재하지 않는지 확인한다. 서브쿼리에 존재하지않는 행을 반환한다

     SELECT a. * FROM  t1 a WHERE NOT EXISTS (SELECT 1 FROM t2 x WHERE x.c1 = a.c1
##### 사용기준
|  |비상관 서브 쿼리|상관 서브 쿼리|
|--|--|--|
|단일 행|비교 조건|비교조건|
|다중 행|IN조건, NOT IN 조건|EXISTS 조건, NOT EXISTS조건|
###### 단일 비상관 서브쿼리
단일 행 비상관 서브쿼리는 메인 쿼리에 단일 값을 입력 할때 사용한다.

    SELECT 고객번호, 고객명
	    FROM 고객
	WHERE 고객번호 = (SELECT MAX (고객주문번호)KEEP(DENSE_RANK FIRST ORDER BY 주문일자 DESC) FROM 주문);

###### 단일 행 상관 서브 쿼리
단일 행 상관 서브 쿼리는 메인 쿼리의 열을 서브 쿼리의 결과와 비교할 때 사용한다.

    SELECT a.고객번호, b.주문번호, b.주문일자
	    FROM 개인고객 a, 주문 b
	WHERE b.주문고객번호 = a.고객번호
위의 쿼리는 고객번호 별로 최종 주문번호와 주문일자를 조회한다. 점 이력 데이터의 최종 이룍을 조회하는 전형적인 기법이다. 개인고객(a)-> 주문(x)->주문(b)순서로 조인된다
###### 다중행 서브 쿼리
다중행 서브쿼리는 원래의 의미에 맞게 사용하면 된다. 내부적인 동작은 신경쓸필요가 없다.

|조건|의미|
|--|--|
|IN 조건|서브쿼리를 먼저조회하여, 메인쿼리에 값을 공급|
|EXISTS 조건|메인쿼리를 먼저 조회하여 서브쿼리로 존재여부 확인|

    SELECT deptno, dname
	    FROM dept
	WHERE deptno IN (SELECT deptno FROM  emp);
	==========================================
	SELECT a.deptno, a.name
		FROM dept a
	WHERE EXISTS (SELECT 1 FROM emp x WHERE x.deptno = a.deptno)
위의 쿼리는 사원이 소속된 부서를 조회하고, 아래쪽 쿼리는 '사원이 존재한는 부서'를 조회한다. 결과는 같지만 의미는 다르다.
##### 인라인 뷰
인라인 뷰는 FROM절에 사용하는 서브쿼리이다. 뷰는 데이터베이스에 저장하는 SELECT문을 테이블처럼사용할수있는 객체이다. 인라인 뷰는 쿼리에서 즉시 처리되는 뷰를 이야기한다.
뷰는 집합 여부에 따라 단순뷰와 복잡뷰로 구분된다.
|뷰|설명|
|--|--|
|단순 뷰|결과 집합의 변경이 없음|
|복잡 뷰|결과 집합의 변경이 있음 (DISTINCT 키워드,GROUP BY 절)|
##### 사용기준
|유형|조인치수|방식|
|--|--|--|
|조인|조인기준의 행이 줄어들거나 늘어날수있음|테이블을 연결하는 기본 방식이다|
|중첩 서브쿼리|메인쿼리의 행이 줄어들수 있지만 늘어나지는 않음|서브쿼리로 메인 쿼리의 결과를 제한 할 때 사용한다.|
|스칼라 서브쿼리|메인 쿼리의 행이 변하지 않음|서브쿼리로 단일 값을 조회할때 사용한다.|
|인라인 뷰|메인 쿼리의 행이 줄어들거나 늘어날수있음 (조인과 동일)|복합는 인라인 뷰로 새로운 결과 집합을 만들거나 조인차수를 1:1 로 만들때 사용한다.|
##### WITH 절
 WITH 절을 사용하면 서브쿼리를 별도의 절에 기술할수있다.
###### SUBQUERY FACTORING절
SUBQUERY FACTORING절을 사용하면 서브쿼리에 이름을 부여하고 이름이 부여된 서브쿼리를 메인쿼리에서 반복 사용할수있다.

    WITH query_name AS (subquery)
	    [, query_name AS (subquery)]...
	SELECT * FROM query_name;
###### PL/SQL 선언 
12.1 버전부터 WITH 절에 PL/SQL함수와 프로시저를 선언할 수있다.
### 집합 연산자 

    SELECT TABLE t1 PURGE;
    SELECT TABLE t2 PURGE;
	
	CREATE TABLE t1 (c1 VARCHAR2 (1) NOT NULL, c2 NUMBER);
	CREATE TABLE t2 (c1 VARCHAR2(1) NOT NULL,c2 NUMBER);
	
	INSERT INTO t1 (c1,c2) VALUES ('A',1);
	INSERT INTO t1 (c1,c2) VALUES ('A',2);
	INSERT INTO t1 (c1,c2) VALUES ('B',1);
	INSERT INTO t1 (c1,c2) VALUES ('B',2);
	INSERT INTO t1 (c1,c2) VALUES ('Z',NULL);
	INSERT INTO t2 (c1,c2) VALUES ('B',1);
	INSERT INTO t2 (c1,c2) VALUES ('B',1);
	INSERT INTO t2 (c1,c2) VALUES ('B',2);
	INSERT INTO t2 (c1,c2) VALUES ('C',2);
	INSERT INTO t2 (c1,c2) VALUES ('Z',NULL);
	COMMIT;
입력데이터를 표로 나타내면 다음과 같다.
|T1.C1|T1.C2|T2.C1|T2.C2|
|--|--|--|--|
|A|1|||
|A|2|||
|B|2|B|1|
|||B|1|
|B|2|B|2|
|||C|2|
|Z|NULL|Z|NULL|
#### 기본 문법
UNION ALL, UNION,INTERSECT,MINUS 연산자가있다.
##### UNION ALL 연산자
UNION ALL 연산자는 데이터을 집합을 수평으로 연결한다. 

    SELECT c1 FROM t1
    UNION ALL
    SELCT c1 FROM t2;
    -----------------------
    C
	-
	A
	A
	B
	B
	Z
	B
	B
	B
	C
	Z
##### UNION 연산자
UNION 연산자는 중복값이 제거된 합집합을 생성한다. 중복값을 제거하기위해 소트가 발생한다.

    SELECT c1 FROM t1
    UNION
    SELECT c1 FROM t2;
    ------------------------
    C
	=
	A
	B
	C
	Z
##### INTERSECT 연산자 
INTERSCT 연산자는 중복값이 제거된 교집합을 생성한다. INTERSECT 연산자도 소트가 발생한다.

    SELECT c1 FROM t1
    INTERSECT
    SELECT c1 FROM t2;
    --------------------------
    C         C2
	- ----------
	B          1
	B          2
	Z
##### MINUS 연산자
MINUS 중복값이 제거되어있는 차집합을 생성한다. MINUS 연산자도 소트가 발생한다.

    SELECT c1 FROM t1
    MINUS 
    SELECT c1 FROM t2;
    
    ----------------------
    
	C
	-
	A
	
	=========================================================================
    SELECT c1,c2 FROM t1
    MINUS 
    SELECT c1,c2 FROM t2;
    
    ----------------------
    
	C         C2
	- ----------
	A          1
	A          2
    
#### 주의 사항
- 연결되는 열의 개수가 다르면 에러가 발생한다
- 연결되는 열의 데이터타입이 다르면 에러가 발생한다.
- 집합연산자를 사용한쿼리는 ORDER BY 절을 쿼리의 마지막에  1번 기술해야한다.
- UNION ALL 연산자는 인라인 뷰에서 데이터를 정렬한 후 데이터를 연결할 수 있다.
- 집합 연산자는 우선 순위가 동일하기때문에 기술 순서대로 연산이 수행된다.(괄호를 사용하면 연산 순서를 조정할 수 있다.)
#### 사용 예제
- OR조건을 사용한 쿼리는 다수의 조건으로 인해 성능이 저하될 수있다. UNION ALL 연산자를 사용하면 데이터 집합을 분리합으로써 쿼리의 성능을 개선할 수있다.
- UNION ALL연산자를 사용하면 조거느이 성능을 개선 할 수있다.
- FULL OUTER JOIN을 수행하면 조인이 여러번수행되어 쿼리의 성능이 저하되었지만 UNION ALL 연산자로 변경하는 기법을 사용 할 수있다.
- UNION, INTERSECT,MINUS 연산자는 대량의 데이터에 소트를 발생하면 쿼리의  성능이 저하될 수있다. IMTERSECT,MINUS연산자는 서브쿼리의 변경을 통해서 소트의 발생을 줄일수있다.

> SORT : 정렬은 항목들을 체계적으로 정리하는 과정으로, 두 가지의 특성이 있으나 그 의미는 구별된다: 순서를 정하는 것 분류하는 것
### 분석 함수
분석 함수는 집계함수의 확장기능으로 생각할수 있다. 집계함수는 햅 그룹으로 값을 집계하고, 분석함수는 파티션과 윈도우로 값을 집계한다. 집계함순느 행 그룹 별호 단일행을 발생하기 때문에 데이터의 집합은 변경되지만, 분석함수는 데이터의 집합을 변경하지 않고 값을 집계한다.
#### 기본 문법
분석함수는 OVER키워드를 사용한다.OVER키워드에 ANALYTIC절을 기술할수있다.

    analytic_function([arguments])over(analyric_clause)
ANALYTIC절은  QUERY PARTITION절,  ORDER BY 절 , WINDOW절로 구성된다.
##### QUERY PARTITION절
expr로 파티션을 지정할 수있다. QUERY PARTITION절은 GROUP BY절과 유사하게 동작한다.파티션은 행그룹과 유사하다. 분석을 위한 정적르룹으로 생각할 수 있다.

    PARTITION BY expr [,expr]..
##### ORDER BY 절
ORDER BY절로 파티션 내의 정렬순서를지정할 수있다.

    ORDER BY expr [ASC|DESC] [NULLS FIRST | NULLS LAST] [,expr [ASC|DESC] [NULLS FIRST | NULLS LAST]]...
##### WINDOWING절
WINDOWING절로 파티션의 윈도우를 지정할 수있다. 윈도우는 현재행의 위치에따라 변경될 수있다.윈도우는 파티션대의 동적 그룹으로 생각할 수 있다.

    {ROWS | RANGE}
	    {BETWEEN{UNBOUNDED PRECEDING | CURRENT ROW | value_expr {PRCEDING | FLOWING}}
	    AND {UNBOUNDED PRECEDING | CURRENT ROW | value_expr {PRCEDING | FLOWING}}
	    | {UNBOUNDED PRECEDING | CURRENT ROW | value_expr {PRCEDING }}}
|  |윈도우 기준|동일한 정렬값|vlaue_expr 계산|
|--|--|--|--|
|ROWS|물리적인 행|다른값을 반환|정렬 행의 위치|
|RANGE|논리적인 범위|같은 값을 반환|정렬값|
윈도우의 범위를 지정하는 키워드이다.

|범위|설명||
|--|--|--|
|BETWEEN ... AND ...|윈도우의 시작과 끝|
|UNBOUNDED PRECDING|앞쪽 끝|
|UNBOUNDED FOLLOWING|뒤쪽 끝|
|CURRENT ROW|현재행|
|value_expr PRECEDING|ROWS|현재 행에서 앞족으로 value_expr 만큼 이동|
||RANGE|현재값에서 valus_expr을 가감|
|value_expr FOLLOWING|ROWS|현재 행에서 뒤쪽으로 value_expr 만큼이동|
||RANGE|현재 값에서 valuw_expr을 가감|

RNAGE 방식에 vlaue_expr 를 사용하면 현재값에서 value_expr을 가감한다
||PRECEDING|FOLLOWING|
|--|--|--|
|오름차순(ASC)|감산|가산|
|내림차순 (DESC)|가산|감산|
  

    DROP TABLE t1 PURGE;
    CREATE TABLE t1 (c1 NUMBER, c2 NUMBER);
	
	INSERT INTO t1 VALUES (1,1);
	INSERT INTO t1 VALUES (2,1);
	INSERT INTO t1 VALUES (3,2);
	INSERT INTO t1 VALUES (4,2);
	INSERT INTO t1 VALUES (5,3);
	INSERT INTO t1 VALUES (6,3);
	INSERT INTO t1 VALUES (7,4);
	INSERT INTO t1 VALUES (8,5);
	INSERT INTO t1 VALUES (9,6);
	COMMIT;
	=============================
	select c1,c2 from t1;

        C1         C2
	---------- ----------
         1          1
         2          1
         3          2
         4          2
         5          3
         6          3
         7          4
         8          5
         9          6


    SELECT C1, C2
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r01
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r02
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r03
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r04
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r05
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r06
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r07
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r08
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r09
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r10
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r11
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r12
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r11
	    ,COUNT (*) ONER (ORDER BY C2 ROWS BETWEEN UNBOUNDED PRECEDING AND 2 PRECEDING) AS r12
	    ,COUNT (*) ONER (ORDER BY C2 ROWS UNBOUNDED PRECEDING) AS r13
	    ,COUNT (*) ONER (ORDER BY C2 ROWS 2 PRECEDING) AS r14
	    ,COUNT (*) ONER (ORDER BY C2 ROW CURRENT ROW) AS r15
	    FROM T1
	ORDER BY 1;
