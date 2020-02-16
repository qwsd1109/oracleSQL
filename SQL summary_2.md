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
 ### 단일 행 함수

|함수|설명|
|--|--|
|단일 행 함수|단일 행을 입력받아서 단일행을 반환하는 함수|
|집계 함수|다중 행을 입력받아 단일 행을 반환하는 함수  |
|분석 함수|다중행 을 입벽받아 다중행을 반환하는 함수|
|모델 함수|MODEL 절 에서 사용하는 함수|

여기서 단일 행 함수는 아래와 같이 구분된다. 

|함수|설명|
|--|--|
|문자 함수|문자 값을 조작하는 함수|
|숫자 함수|숫자 값을 조작하는 함수|
|날짜 함수|날짜 값을 조작하는 함수|
|변환 함수|값의 데이터타입을 조작하는 함수|
|널 관련 함수|널을 조작하는 함수|
|비교 함수|값을 비교하는 함수|
|엔코딩 디코딩 함수|값을 조사하거나 디코딩하는 함수|
|홤경 식별자 함수|인스턴스와 세션에 대한 정보를 제공하는 함수|
|계층 함수|계층쿼리에서 사용하는 함수|
|컬랙션함수|컬렉션 값을 조사하는 함수|
|XML 함수|XML 값을 조작하는 함수|
|JSON 함수|JSON값을 조작하는 함수|
#### 문자함수
##### CHR 함수 
CHR 함수는 n에 해당되는 데이터베이스 캐릭터 셋의 문자 값을 변환한다.

    CHR (n)
    
    select CHR(38) from dual;  
    ------------------------------
    &
##### LOWER 함수
LOWER 함수 는 char를 소문자로 바꾼다

    LOWER (char)
    
    SELECT LOWER('SQL') FROM DUAL;
    -------------------------------
    sql
##### UPPER 함수    
UPPER 함수는 char를 대문자로 변경한다.

    UPPER (char)
    
    SELECT UPPER ('sql') FROM DUAL;
    -------------------------------
    SQL
##### INITCAP 함수
INITCAP 함수는 char가 포함된 첫글자는 대문자, 나머지는 소문자로 변경한다.
 
    INITCAP (char)
    
    SELECT INITCAP ('sqL') FROM DUAL;
    -------------------------------
    Sql 
##### LPAD 함수
LPAD 함수는 expr1의 길이를 좌측으로 n만큼 늘린다음 늘어난공간을 expr2로 반복해서 채운다.

    LPAD (expr1, n[,expr2])
    
    SELECT  LPAD(30, 5) as c1, LPAD(30, 5, ' ')as c2 , LPAD(33, 5, '0')as c3
    FROM DUAL;
    ---------------------------------------------------------------------
    C1     c2     c3
    =====  =====  =====
       30     30  00033
       
##### RPAD 함수
   RPAD 함수는 expr1의 길이를 우측으로 n만큼 늘린다음 늘어난공간을 expr2로 반복해서 채운다.
   
    RPAD (expr1, n[,expr2])
    
    SELECT  RPAD (20, 5) as c1, RPAD (20, 5, ' ')as c2 , 
		    RPAD (32, 5, '0')as c3, RPAD (34, 7, '12')as c4
    FROM DUAL;
    ---------------------------------------------------------------------
    C1     C2     C3     C4
    =====  =====  =====  =======
    20     20     32000  3412121
##### LTRIM 함수
LTRIM 함수는 char의 좌측부터 set에 포함된 문자를 제거한다.  char는 한 문자씩  set문자와 비교되며 ,set문자에 포함되지 않는 문자를 만나면 제거가 중단된다.

    LTRIM (char[, set])
    SELECT LTRIM ('ABC','CB') AS C1,LTRIM ('ABC','AB') AS C2  FROM DUAL;
    ----------------------------------------
    C1    C2
    ====  ====
    A     ABC

##### RTRIM 함수
RTRIM함수는 char의 우측부터 set에 포함된 문자를 제거한다.  char는 한 문자씩  set문자와 비교되며 ,set문자에 포함되지 않는 문자를 만나면 제거가 중단된다.

    RTRIM(char[, set])
    SELECT RTRIM('ABC','CB') AS C1,RTRIM('ABC','AB') AS C2  FROM DUAL;
    ----------------------------------------
    C1    C2
    ====  ====
    ABC   A
##### TRIM 함수
TRIM 함수는 trim_source의 좌측이나 우측이나 양측에서 trim_char를 제거한다, trim_char가아닌 문자를 만나면 제거를 멈춘다.trim_char는 한글자만 지정할 수 있다
**주로 양측공백을 제거하는용도로 사용한다**

    TRIM([{{LEADING||TRAILING||BOTH}[trim_char] | trim_char} FROM ]trim_char)
    
    SELECT 
		    TRIM (LEADING 'A' FORM 'AAB '), AS C1,
		    TRIM (BOTH 'A' FORM 'AAB '), AS C2,
		    TRIM (' AAB '), AS C3,
		    FROM DUAL
	-----------------------------------------------------------
	C1  C2  C3
	--  --  ---
	B 	B   AAB
##### SUBSTR 함수
SUBSTR 함수는 char를 position 위치에서 n 만큼 자른다.n을 생략하면 끝까지 짤린다. position이 음수일경우 우측으로 자른다.

    SUBSTR 9char, position[, n])
    SELECT 
		    SUBSTR ('123456789',4) AS C1,
		    SUBSTR ('123456789',4,4) AS C2,
		    SUBSTR ('123456789',-4) AS C3
	FROM DUAL
	--------------------------------------------
	C1  C2    C3
	=== ===== ====== 
	123 12389 456789
##### REPLACE 함수
REPLACE 함수는 char에 포함된 seach_string을 replacement_string 으로 변경한다.

    REPLACE (char,seach_string[,replacement_string ])
    
    SELECT 
		    REPLACE ('ABCABCABC','AB') AS C1,
		    REPLACE ('ABCABCABC','AB','F') AS C2,
		    REPLACE ('ABCABC','AB','1234') AS C3,
	FROM DUAL
	--------------------------------------------------------
	C1  C2     C3
	=== ====== ==========
	CCC	FCFCFC 1234C1234C   
##### ASCII 함수
ASCII 함수 char 첫 문자의 아스키값을 십진수로 변환한다.

    ASCII(char)
    
    select ASCII(38) from DUAL; 
    ------------------------------
    &
##### INSTR 함수
INSTR 함수는 string의 position에서 우측으로 occurrence번째 substring의 시작위치를 변환한다. 

    INSTR (string, substring [,position[,occurrence]])

    SELECT INSTR('CONGRATULATIONS', 'AT', 3, 2) FROM DUAL
- 'CONGRATULATIONS' 문자열에서 'AT' 문자열을 세번째문자(N)부터 찾아서 'AT'문자열이 두번째 나오는 위치를 리턴하라. 두번째 'AT'에서 'A'의 위치는 열번째이므로 10이 리턴.
##### LENGTH 함수
LENGTH 함수는 char의 길이를 반환한다.

    LENGTH (CHAR)
    
    SELECT LENGTH ('QWERTYUIOP')FROM DUAL;
    ---------------------------------------
    10
#### 숫자 함수 
##### ABS  함수
ABS  함수는 절대값을 반환한다.

    ABS (n)
    
    SELECT ABS (-43895) FROM DUAL;
    ------------------------------------
    43895
##### SIGN 함수 
SIGN 함수는 N 의 부호를 나타낸다 양수면 1, 음수면 -1,0이면 0을 반환한다.

    SIGN(N)
    
    SELECT SIGN(-22131) FROM DUAL;
    --------------------------------
    -1
##### ROUND 함수
ROUND 함수는 N1을 N2 자리로 반올림한다. N2 가 양수면 소수부, 음수면 정수부를 반올림한다.

    ROUND (N1[,N2])
    SELECT ROUND (15.59) AS C1, 
		   ROUND (15.59,1) AS C1, 
		   ROUND (15.59,-1) AS C3 
	FROM DUAL;
    ---------------------------------------------------------
	C1 C2   C3
	== ==== ==
	16 15.6 20
##### TRUNC 함수
 TRUNC 함수는 N1 을 N2자리로 버린다. N2 가 양수면 소수부, 음수면 정수부를 버린다.
 
    TRUNC (N1[,N2])
    SELECT TRUNC (15.59) AS C1, 
		   TRUNC (15.59,1) AS C1, 
		   TRUNC (15.59,-1) AS C3 
	FROM DUAL;
    ---------------------------------------------------------
	C1 C2   C3
	== ==== ==
	15 15.5 10
##### CEIL 함수
CEIL 함수는 N보다 크거나 같은정수의 최소값을 반환한다.

    CEIL(N)
    
	SELECT CEIL (1.5) FROM DUAL;
	---------------------------------
	2
##### FLOOR 함수
FLOOR 함수는 N보다 작거나 같은 정수를 반환 한다.

    FLOOR (N)
	
	SELECT FLOOR (15.5) FROM DUAL;
	-------------------------------------------
	15
##### MOD 함수
MOD 함수는 n1을 n2로 나눈 나머지를 반환한다. n2가 0 이면 n1을 반환한다.

	MOD (n1,n2)
	
	SELECT 
		MOD (11,4) AS C1, 
		MOD (11,0) AS C2,
		MOD (11,-4)AS C3 
	FROM DUAL;	
	-------------------------------------
	C1  C2  C3
	=== === ===
	  3  -3  11
##### REMAINDER 함수
REMAINDER 함수는 N1을 N2 로 나눈 나머지를 반환한다 N2가 0 이면 에러 발생한다.

    REMAINDRE(N1,N2)
	
	SELECT 
		REAMINDER (11,4) AS C1, 
		REAMINDER (11,-4) AS C2
	FROM DUAL;	
	-------------------------------------
	C1  C2 
	=== === 
	 -1 -1
##### POWER 함수
POWER함수는 N1을 N2로 거듭 제곱한 값을 반환한다.

    POWER (N1,N2)
	
	SELECT POWER (3,8) FROM DUAL;
	---------------------------------------
	6561
##### BITAND 함수 
BITAND 함수는 N1 와 N2 의 비트 AND연산 값을 변환한다.

    BITAND(N1,N2)
	
	SELECT BITAND (12,6)FROM DUAL;
	---------------------------------
	4
##### WIDTH_BUCKET 함수
WIDTH_BUCKET 함수는 min_value ~ max_vlaue의 범위 에 대해 num_buckets개의 버킷을 생성한후 expr이 속한 버킷을 반환 한다,쉽게 말하자면 expr 개의 바구니를 생성한뒤에 바구니 만에 각각의 균등하게 나누어진 범위에 따라 바구니의 등급 값이 반환이 된다.

    WIDTH_BUCKET (expr, min_value, max_value, num_buckets)
	
	------------------------------------------------------
##### 그 밖에 함수들
| 함수 |설명  |
|--|--|
| SORT (n) | n의 제곱근을 반환 |
|EXP(n) |e의 n 제곱을 반환 |
|LN(n) | n의 자연 로그 값을 반환|
|LOG(n1,n2) |밑을 n1으로하는 n2의 로그 값을 반환 |
|SIN(n) | n의 사인 (sine) 값을 반환|
|ASIN(n) |n의 역 사인 (arc sine) 값을 반환 |
|SINH(n) |n의 쌍곡선 사인 (hyperbolic sine) 값을 반환 |
|COS(n) |n의 코사인 (cosine) 값을 반환 |
|ACOS(n) |n의 역 코사인 (arc cosine) 값을 반환 |
|COSH(n) | n의 쌍곡선 코사인 (hyperbolic cosine) 값을 반환|
|TAN(n) |n의 사인 (tangent) 값을 반환 |
|ATAN(n) |n의 역 탄젠트 (arc tangent) 값을 반환 |
|ATAN2(n1,n2) |n1 / n2의 역 탄젠트 (arc tangent) 값을 반환 |
|TANH(n) |n의 쌍곡선 탄젠트 (hyperbolic tangent) 값을 반환 |

#### 날짜 함수
##### SYSDATE 함수
SYSDATE 함수는 초가 포함된 데이터 베이스 서버의 날짜값을  DATE 타입을 변환한다.

    SYSDATE

	SECLET SYSDATE FROM DUAL;
	-----------------------------------
	
##### SYSTIMESTAMP 함수
SYSTIMESTAMP함수 는 소수점이라의 초가 포함된 데이터베이스 서버의 날짜 값을 TIMESTAMP WITH TIME ZOME 타입으로 변환한다.

    SYSTIMESTAMP
	
	SELECT SYSTIMESTAMP FROM DUAL;
	
##### NEXT_DAY 함수
NEXT_DAY 함수는 DATE 이후 CHAR 에 지정된 요일에 해당하는 가장 가까운 날짜값을 변환한다.

|일|월|화|수|목|금|터|
|--|--|--|--|--|--|--|
|1|2|3|4|5|6|7|



    NEXT_DAY(DATE,CHAR)
    SELECT NEXT_DAY(DATE '2050-01-01' , '2 ')
    ---------------------------------------------
    2050-01-03 00:00:00
##### LAST_DAY함수
LSAT_DAY 함수는 DATE가 속한 월의 말일을 뜻한다

    lAST_DAY (DATE)

	SELECT LAST_DAY(DATE'2020-02-05')FROM DUAL;
	------------------------------------------
	2020-02-29
##### ADD_MONTHS 함수
ADD_MONTHS는 DATE 에서 INT의 개월 수를 가감한 날짜를 반환한다.

    ADD_MONTHS(DATE,INT)
	
	SELECT ADD_MONTHS(DATE'2020-02-05')FROM DUAL;
	---------------------------------------------------
	2020-01-05 00:00:00
##### MONTH_BETWEEN 함수
 MONTH_BETWEEN 함수는 DATE1과 DATE2 사이의 개월수를 변환한다.
 

     MONTH_BETWEEN (DATE1,DATE2)
##### EXTRACT 함수
EXTRACT 함수는 EXPR에서 날짜를 추출한다.

    EXTRACT({YEAR|MONTH|DAY|HOUR|MINUTE|SECOND}FROM expr)
##### ROUND (date) 함수 
ROUND 함수는 FMT을 기준으로 DATE를 반올림한다.

    ROUND (DATE[,FMT])

##### TRUNC 함수
TRUNC 함수는 FMT을 기준으로 DATE를 버린다. 

    TRUNC (DATE[,FMT])
### WHERE 절
 
    SELECT 절 -- (3)
    FROM 절 -- (1)
    WHERE 절 -- (2)
#### 비교 조건
|비교 조건|설명|
|--|--|
| = | 같음 |
| <>,!=,^= | 다름 |
| > | 큼 |
| < | 작음 |
| >= | 크거나 같음 |
| <= | 작거나 같음 |
| ANT,SOME | 목록 일부를 비교 |
| ALL | 목록 전체를 비교 |

    SELECT  ename, sal FROM emp WHERE job ='ANNLYST';
    SELECT  ename, sal FROM emp WHERE sal>2500;
    SELECT  ename, sal FROM emp WHERE hiredate < DATE '1960-02-17';
웨의 행들은 job이 ANNLYST인 행을 조회하거나 , 2500 보다 큰행을 조회하거나 1960-02-17날짜의 이전의 것을 조회한다.

    SELECT  ename, sal FROM emp WHERE comm<> 0;
이 구문은 0이 아닌 구문을 조회하는 경우 사용된다.

    SELECT  ename, sal FROM emp WHERE ANY(1000,2500);
ANY 조건문은 두개의 구문중에서 하나만 만족할경우 행을 반환한다.

    SELECT  ename, sal FROM emp WHERE ALL (1000,2500)
ALL조건 구문은 목록전체를 비교하여 조건을 만족하는 행을 반환한다.
#### 논리 조건
![AND OR NOT 이미지 검색결과](https://img1.daumcdn.net/thumb/R720x0.q80/?scode=mtistory2&fname=http%3A%2F%2Fcfile2.uf.tistory.com%2Fimage%2F2651CC47535F88161CBB74)

	SELECT  ename, deotno, job FROM emp WHERE deptno = 30 AND job = 'CLERK';
위에 쿼리는 deptno 가 30이면서  job 이 CLERK 인 행을 반환한다.

	SELECT  ename, deotno, job FROM emp WHERE deptno = 30 OR job = 'CLERK';
   위에 쿼리는 deptno 가 30이거나  job 이 CLERK 인 행을 반환한다.
   
   	SELECT  ename, deotno, job FROM emp WHERE NOT (deptno = 30 OR job = 'CLERK');
위에 쿼리는 deptno 가 30이거나  job 이 CLERK이 아닌 행을 반환한다.
#### BETWEEN 조건
BETWEEN 조건은 값의 점위에 해당하는 행을 반환한다.

    expr1 [NOT] BETWEEN expr2 AND expr3
아래의 쿼리는 2500이상 3000이하의 행을 조회한다.

    SELECT ename, sal FROM emp WHERE sal BTWEEN 2500 AND 3000;
    ==
    SELECT ename, sal FROM emp WHERE sal >=2500 AND sal <=3000;
아래의 쿼리는 2500 이상, 3000이하가 아닌 행을 조회한다.

    SELECT ename, sal FROM emp WHERE sal NOT BTWEEN 2500 AND 3000;
#### IN조건
IN조건은 expr목록과 일치하거나 일치하지 않는 행을 반환한다.

    expr [NOT] IN (expr[,expr])
아래의 쿼리는 ANALYST이거나 MANAGER인 행을 조회한다.

    SELECT ename, job FROM emp WHERE job IN (ANALYST,MANAGER);

 IN 조건은 다중열로도 가능하다. 아래의 쿼리는 deptno 가 10이고 job이 MANAGER인 행이거나, deptno 가 20이고 job이 ANALYST인 행을 조회한다.
 

    SELECT ename, deptno, job 
    FROM emp 
    WHERE (deptno, job) IN ((10,MANAGER),(20,ANALYST));
아래의 쿼리는 ANALYST이거나 MANAGER가 아닌 행을 조회한다.

    SELECT ename, job FROM emp WHERE job NOT IN (ANALYST,MANAGER);
#### LIKE 조건
LIKE 조건은 char1이 char2 패턴이 일치라는 행을 반환한다.

    char1 [NOT] LIKE char2 [ESCAPE esc_char]
|특수문자|설명|
|--|--|
|%|0개 이상의 문자와 일치|
|_|하나의 문자와 일치|

    SELECT ename FROM  emp WHERE LIKE 'A%';
위의 쿼리는 이름이 A로 시작하는 행을 조회한다.

    SELECT ename FROM  emp WHERE LIKE 'A%S';
위의 쿼리는 이름이 A로 시작되어 S로 끝나는 행을 조회한다.

    SELECT ename FROM  emp WHERE LIKE '%ON%';
    
위의 쿼리는 이름안에 ON이 포함된 글자를 조회한다.

    SELECT ename FROM  emp WHERE LIKE '__N__';
위에 쿼리는 글자가 5글자면서 중간에 N이 포함되는 쿼리는 반환한다.

    SELECT ename FROM  emp WHERE NO LIKE '%A%';
위의 쿼리는  A가 포함되어있지 않는 행을 조회한다.
#### 널 조건
널조건은 널을포함하거나 널을 포함하지 않는 행을 조회한다.

    expr IS [NOT] NULL
아래의 쿼리는 comm이 널이 쿼리를 조회한다.

    SELECT ename , comm FROM emp WHERE comm IS NULL;

아래의 쿼리는 널이 아닌 행을 조회한다.

    SELECT ename , comm FROM emp WHERE comm IS NOT NULL;
아래의 쿼리는 comm이 널이거나 0인행을 조회한다.

    SELECT ename,comm FROM emp WHERE (comm IS NULL OR comm = 0);

 ##### LNNVL 함수
 LNNVL 함수는 condition이 FLASE 나 UNKNOWN이면 TRUE,TRUE면 FALSE를 반환한다.
 

    LNNVL (condition)
#### 조건의 우선순위
|우선순위|조건|
|--|--|
|1|연산자|
|2|비교조건|
|3|IN조건,LIKE조건,BTWEEN조건,널조건|
|4|논리조건(NOT)|
|5|논리조건(AND)|
|6|논리조건(OR)|
### ORDER BY 절
order by 절은  조회한 데이터들을 목적에 맞게 특정 칼럼을 기준으로 '정렬'하는데 사용된다.  
  
    SELECT 절 -- (3)
    FROM 절 -- (1)
    WHERE 절 -- (2)
    ORDER BY --(4)
    
#### 기본 문법
ORDER BY 구문 

    ORDER BY {expr | position | c_alias} [ASC | DESC] [NULLS FIRST | NULLS LAST] 
    [,{expr | position | c_alias} [ASC | DESC] [NULLS FIRST | NULLS LAST] ]
|항목|설명|
|--|--|
|ASC|오른차순으로 정렬(기본 값)|
|DESC|내린차순으로 정렬|
|NULLS FIRST|널을 앞쪽으로 정렬 (내린차순 정렬 시 기본값)|
|NULLS LAST|널을 뒤쪽으로 정렬 (오름차순 정렬 시 기본값)|

    SELECT ename, sal, comm FROM emp WHERE deptno = 30 ORDER BY sal DESE ,comm
위에 쿼리는 sal은 내림차순, comm은 오름차순으로 결과를 정렬한다.
#### 활용 예제
##### 조건 정렬
ORDER BY절에 DECOD E함수나 CASE표현식을 사용하면 조건에 따라 다른 정렬기준을 지정할 수 있다.
 
