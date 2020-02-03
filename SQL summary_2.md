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
	    

 
