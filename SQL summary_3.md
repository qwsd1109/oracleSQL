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


