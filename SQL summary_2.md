# 불친절한 SQL 프로그래밍
## 기초쿼리
### SELECT문 
**SELECT문은 데이터를 조회하는 구문이다**
기본 SELECT문은 SELECTDHK FROM절로 구성된다. FROM 절 실행한이후 SELECT문을 수행한다.

    SELECT 절 -- (2)
    FROM 절 -- (1)
    
|절|설명|
|--|--|
|SELECT절|조회할 열이나 표현식 기술|
|FROM절|조회할 테이블 기술|
#### SELECT절
##### 에스터리스크 (*)
SELECT절에 있는 에스터리스크(Asterisk *)를 기술하면 전체열이 조회된다. 

    SELECT * FROM stdent;

>   DESC 명령어 는 테이블의 열정보를 확인할수있다.
>       `DESC stdent`

 ##### 열
 SELECT절에 조회항 열을 기술할수있다. 열은 ,(쉼표)로 구분한다.
 테이블의 열 순서와 상관없이 지정할 수 있다.
 

    SELECT Sname, Sno FROM stdent; == SELECT stdent.Sname, stdent.Sno FROM stdent;
  두 구문은 같다.
  

> 에러 메세지
> 오라클 데이터 베이스는 에러발생시 에러코드와 에러메세지를 반환한다. 데이터서버에서 oerr명령어를 사용해도 에러의 내용을 알 수 있다.
> `oerr ora 923`
##### 열 별칭
열은 별칭을 지정할 수 있다 별칭을 지정하면 열이나 표현식을 간결하게 사용할 수 있다. 별칭은 대소문자를 구분하지않고 숫자로 시작할 수 없으며, 공백이나 특수문자 (#,$,_ 는 제외 )를 포함할 수 없다. 

    ㄴSELECT Sname stdent_name,Sno AS stdent_no FROM stdent
##### DISTINCT 키워드
SELECT 절에 DISTINCT 키워드나 UNIQUE키워드를 기술하면 중복되는 행이 제거되는 결과가 반환된다.기본값은 ALL이다

    SELECT DISTINCT Sno FROM stdent;
    
#### FROM절
FROM 절에서는 조회할 테이블을 기술할 수 있다. 테이블은 ,(쉽표)로 구분된다.2개 이상의 태이블이 있을시에 조인이 수행된다.
##### 스키마 
테이블에 스키마를 지정하면 해당스키마의 테이블을 조회할 수 있다. 열은 테이블,테이블은 스키마에 종속구조이다.
##### 테이블 별칭
테이블도 별칭을 지정할 수 있다.테이블별칭으로 열을 한정하면 퀴리의 가독성을 높일 수 있다.

    SELECT S.Sname FROM stdent S;
##### SAMPLE절
SAMPLE절을 사용하면 테이블을 샘플링하여 조회할 수 있다. 대용량 테이블에 대한 통계 값을 생성하고 활용할떄 사용할 수 있다.

    SAMPLE [BLOCK] (sample_percent)[SEED(seed_vale)]

|항목|설명|
|--|--|
|BLOCK|블럭 샘플링을 사용|
|sample_percent|샘플링 비율|
|SEED(seed_vale)|항상동일한 비율의 샘플 반환|
#### 기본요소
##### 리터럴
리터럴은 변하기 않는 값이다.문자 ,숫자,날짜, 인터벌 리터벌을 사용할 수 잇다.
###### 문자 리터럴
문자 리터럴은 문자값을 지정한다. 문자열은 작은 따움표(')로 감싸서 기술한다.

    SELECT q'[2'B]' AS c1, q'{[3C]}' AS c2 FROM DUAL;
    

> DUAL 테이블
> DUAL 테이블은 dummy열로 구성되며, 1개의 행을 가지고있다. 리터럴조회, 행복제 등의 다양한 용도로 활용될 수 있다.
######  숫자 리터럴
숫자 리터럴은 숫자 값을 지정한다. E는 과학적인 표기법을 나타낸다.

    SELECT 1 AS c1, -2 AS c2,3.4 AS c3,-5.6 AS c4, 1.2E2 AS C5 FROM DUAL;

> COL[UMN]명령어
> COL[UMN] 명령어로 열 포멧을 설정할 수 있다. 
> `COL[UMN] column FOR[MAT] format`
###### 날짜 리터럴 
날짜 리터럴은 날짜값을 지정한다. 날짜 값은 NLS 파리미터 설정에 따라 출력포멧이 설정된다. 
|NLS파라미터|값  |
|--|--|
|NLS_DATE_FORMAT|YYYY-MM-DD HH24:MI:SS|
|NLS_TIMESTAMP_FORMAT|YYYY-MM-DD HH24:MI:SS.FF|
|NLS_TIMESTAMP_TZ_FORMAT|YYYY-MM-DD HH24:MI:SS.FF TZH:TZM|
###### 인터벌 리터벌
인터벌 리터럴은 시간 간격을 지정한다. YEAR TO MONTH, DAY TO SECOND 리터럴을 사용할 수 있다.
##### 널(NULL)
널(NULL)은 값이 없거나 정해지지않은것을 의미한다. 오라클 인터페이스에서는 널과 빈 문자(' ')를 동일하게 처리한다.

    SELECT NULL AS c1, '' AS c2 FROM DUAL;
   NULL키워드를 사용하는것이 가독성측면에서 좋다.
##### 연산자 
연산자는 피연산자에 대한 연산을 수행한다. 오라클 데이터베이스는 다양한 연산자를 제공한다. 
##### 표현식
표현식은 값을 평값으로 평가될 수 있는 리터럴,연산자 ,SQL 함수 등의 조합이다. 오라클데이터베이스는 다양한 표현식을 제공한다.
###### CASE 표현식
CASE표현식을 사용하면 IF THEN ELSE 논리를 평가할 수 있다. 단순 CASE 표현식과 검색 CASE 표현식을 사용할 수 있다.

- 단순 CASE표현식은 expr 와 comparison 이 일치하는 첫번째 return_expr,일치하는comparison이 없으면 else_expr을 반환한다.
`CACE expr 
	{WHEN comparison_expr THEN return_expr}... 
	[ELSE else_expr] 
END`
- 검색 case 표현식은 condition이 TRUE인 첫번째 return_expr를 반환한다. TRUE 인 condition가 없으면 else_expr가 반환된다.
`CASE 
	{WHEN condition THEN return_expr}... 
	[ELSE else_expr] 
END`
##### 슈도칼럼
슈도칼럼은 테이블에 저장되있지 않는 의사 칼럼으로 쿼리 수행시점에 값이 결정된다.
|구문|슈도 칼럼|장|
|--|--|--|
|일방|REWID, ROWNUM, ORA_ORWSCN  |5장, 15장, 19장|
|계층 쿼리|LEVEL, CONNECT_BY_ISLEAF, CONNECT_BY)ISCYCE|16장|
|시퀀스|CURRVAL, NEXTAL|20장|
|버전 쿼리|VERSIONS_STARTSCN, VERSIONS_STARTTIME|31장|

    SELECT Sname, R0WID AS c1 FROM stdent;
c1열이 저장된 주소를 반환한다.
##### 주석
SQL도 다른 프로그래밍언어처럼 주석을 기술할 수 있다.
|유형|문법|
|--|--|
|단일 행 주석|-로 시작함|
|다중 행 주석|/*로시작해서 */로 끝남|
###### 힌트
힌트는 옵티바이저에 명령을 전달하는 특별한 형태의 주석이다. 대부분은 실행 계획 수립할 때 사용되지만 이부는 SQL문 동작을 제어한다. 
|유형|문법|
|--|--|
|단일 행 주석|--+로 시작함|
|다중 행 주석|/*+로시작해서 +*/로 끝남|
    

> V$SQL_HINT뷰
>V$SQL_HINT뷰에서 힌트를 조회할수 있다. 
>`SELECT name, inverse, version FROM v$sql_hint;`
##### 바인드 변수
SQL 도 바인드 변수를 사용할 수 있다. 바인드 변수를 사용하면 쿼리의 재사용성을 높일 수 있다.

    VAR v1 NUMBER;
    EXEC : v1 :=1;
    SELECT :v1 AS c1 FROM DUAL;
