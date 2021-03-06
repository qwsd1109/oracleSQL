# 불친절한 SQL 프로그래밍
## 기본 개념
### 데이터
####  데이터란?
데이터는 해석할수있는 의미를 가진 기호 라는 뜻을 가지고 있습니다. 과거에는 인간만이 데이터를 기록할 수 있었지만 현제는 인간이 만든사물이 즉 사물인터넷등이  인간보다 많은 데이터를 생성되고 있다.혹은 데이터는 정성적 또는 정량적 변수 값의 집합 으로 해석할 수 있다.

 숫자처럼 양을 측정할 수 있는 데이터를 정량 데이터, 텍스트나 이미지처럼 양으로 측정할수없는 데이터를 정성적 데이터라고 한다. 
 
정량적 데이터는 구조화 되어 저장되기 때문에 정형 데이터 정성적 데이터는 반대의 의미인 비정형적 데이터라 불리운다.

데이터  ,정보  ,지식 , 지혜 는 순환관계를 지니었다. 데이터를 분석하여 정보를 생성하도 , 정보를 해석함으로써 지식을 축적하고 , 축적된 지식에서 지혜를 실천함으로써 새로운 데이터를 발생하는 과정이 반복 된다.

#### 데이터 베이스 
데이터 베이스는 데이터를 정리하여 모아둔것 이다. 데이터는 초기에 파일 시스템으로 관리되었지만, 데이터의 증가로 인해서 효율적으로 관리 할 수 있는 방법이 필요했습니다. 이런 방법은 데이터베이스 모델이라고 한다. 현재는 주로 관계형 모델이 사용되고있습니다.

관계형 모델은 릴레이션에 데이터를 저장한다 릴레이션은 2차원의 표로 구성된다. 릴레이션은 튜플의 집합이고 튜플은 속성의 집합이다.
![릴레이션에 대한 이미지 검색결과](https://t1.daumcdn.net/cfile/tistory/99269B375B3530C51A)
관계형 데이터 베이스를 채택한 데이터 베이스를 관계형 데이터 베이스라고 한다. 
###  데이터 모델링
#### 데이터 모델 
데이터 모델은 현실세계를 데이터베이스로 구축할 수 있도록 **추상화**를 한 것 입니다.**데이터 모델은 상세화 수준에 따라 개념, 데이터 모델, 논리 데이터모델, 물리데이터 모델로 구분된다.** 요구 사항을 분석하여 개념 데이터 모델을 설계하고 데이터 베이스 모델에 따라 개념데이터 모델을 논리 데이터 모델로 상세화 한후, DBMS에따라 논리데이터를 물리 데이터 모델로 전환한다.
#### E-R모델
E-R모델은 엔터티와 관계로 데이터를 표현한다. 
| 개념 | 집합 | 개별 |
|--|--|--|
|어떤것|언터티 타입|엔터티,인스턴스|
|어떤 것의 관계|관계|페어링|
|어떤 것의 특징 |속성 | 속성값 |
- 아래의 표는 E-R모델, 관계형모델, 오라클데이터 베이스의 유사한 용어를 보여준다.

|E-R 모델|관계형 모델| 오라클 데이터 베이스|
|--|--|--|
|엔터티|릴레이션|테이블|
|인스턴스|튜플|행|
|속성|속성|열|
##### 엔터티 
엔터티는 개채를 인식할 수 있는 데이터의 집합이다.
###### 기본 식별자 
기본 식별자는 엔터티에서 인스턴스를 고유하게 식별할 수 있는 속성이다. 엔터티 반드시 기본 식별자를 가져야한다.  
 하나의 속성으로 구성된 식별자를 단일 식별자, 2개이상의 속성으로 구성된 식별자를 복합 식별자라고 한다.
 ##### 속성
 **속성은 엔터티에서 가져야하는 데이터의 최소단위이다.**속성은 분리되지않아야하며 하나의 속성값만 가저야 한다. 
 ###### 도메인
 도메인은 속성값의 범위를 나타낸다.
 

    성별 VARCHAR2(1) NOT NULL CONSTRAINT t1_c1 CHECK (성별 IN ('M','F'))
위에 코드는 길이가 한자리인 문자를 받는 구문이고 NOT NULL은 값이 반드시 존재한다는 뜻이고,  CHECK는 제약 조건을 거는 것으로써 M또는 F 밖에 입력이 되지않는 것을 의미한다.
#### 관계
관계는 엔티티간의 업무적 연관이다 .하나의 엔터티는 하나이상의 엔터티와 관계를 가리수있고, 관계를 가진 엔터티와 또 다른 관계를 가질수 있다.
##### 카디널리티
카디널리티는 하나의 부모 인스턴스가 몇개의 자식 인스턴스와 태어링 될수있는지 나타낸다. ![카디널리티에 대한 이미지 검색결과](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzU0MjExQGZzNS50aXN0b3J5LmNvbTovYXR0YWNoLzAvMC5qcGc%3D)
카디널리티는 1:1 ,1:M,M:M관계를 가질수 있다. 이때 M:M관계는 엔터티를 추가하여 이를 해소해야한다.
##### 옵셔널리티
옵서널리티는 부모인스턴스와 자식 인슽너스간의 페어링 여부를 나타낸다. 페어링되어야하면 필수관계, 페어링되지 않아도 되면 선택관계를 가진다.
![카디널리티에 대한 이미지 검색결과](http://cfs5.tistory.com/upload_control/download.blog?fhandle=YmxvZzU0MjExQGZzNS50aXN0b3J5LmNvbTovYXR0YWNoLzAvMC5qcGc%3D)
##### 관계유형
관계 유형은 식별 관계와 비 식별관계로 구분된다. 식별관계는 부모엔터티의 기본 식별자가 자식 식별자의 기본 속성으로 상속된다. 비식별관계는 일반속성으로 식별된다.
#### 정규형
정규형은 데이터 이상을 제거하기 위해 관계형 모델의 설계지침이다. 
##### 정규화
정규화는 정규형을 위배한 릴레이션을 정구형으로 만드는 과정이다

- 정규형
	- 1정규형은  다중값과 반복 그룹에 대한 내용이다.다중값을 가진 속성을 다가속성으로 부르고, 반복 그룹을 가진 속성은 반복속성으로 부르기도 한다.
		- 1정규형은 데이터모델의 오랜논쟁거리라고 한다 규칙에 얽매이지말고 유연하게 대처하기바람
	- 2정규형 은 부분 종속과 관련이 있다, 부분종속은 일반 속성이 식별자의 일부속성에만 종속되는 것이다. 
	- 3정규형은 이행종속과 관련이 있다. 이행종속은 일반속성이 다른 일반속성에 종속되는 것이다.
##### 반정규화
반정규화는 성능개성을 위해 의도적으로 데이터를 중복시키는 것이다. 정규형을 위배한 릴레이션은 데이터 무결성을 보장하기어렵기떄문에 제한적으로 사용해야한다.
### 오라클 데이터 베이스
#### 개념
오라클 데이터베이스는 오라클사에서 개발한 ORDBMS(객체 관계 데이터베이스)제품이다
##### 사용자
**사용자는 데이터베이스에 로그인 할 수있는 계정이다.** 데이터 베이스를 생성하면 관리자계정이 함께 생성된다.
##### 오브젝트
오브젝트는 논리적인 데이터 베이스 구조이다. 오브젝트는 사용자에 종속될 수 있다. 사용자에 종속된 오브젝트의 논리적 집합을 스키마라고한다. 

사용자에 종속된 오브젝트를 스키마오브젝트, 종속되지않는 오브젝트를 비 스키마 오브젝트라고 한다.
|구분|오브젝트|
|--|--|
| 스키마 오브젝트 | 테이블, 클러스터,인덱스, 뷰,시퀸스,시노님, 오브젝트 타입 |
| 비 스키마 오브젝트 | 사용자, 롤, 디렉터리 |
오브젝트는 데이터 저장 여부에 따라 세그먼트 오브젝트 와 비  세그먼트 오브젝트로 구분할 수 있다. 저장공간이 필요한 오브젝트는 세그먼트라한다 
|구분|오브젝트|
|--|--|
| 세그먼트 오브젝트| 테이블,클러스터,인덱스 |
| 비 세그먼트 오브젝트 | 뷰, 시퀀스,시노님,오브젝트타입, 사용자,롤,디렉터리 |
##### 테이블
테이블은 데이터를 구성하는 기본 단위이다. 행과 열로 구성된다
### 구조 
|항목|오라클 데이터 베이스|엑셀|
|--|--|--|
|파일|data file,control file,online redo iog|excel file  |
|메모리|SGA,PGA|cache|
|프로세스|background process, sever process |process, thread  |
|프로그램|oracle database|office |
|운영체제|unix,linus,windows |windows  |
#### 데이터베이스와 인스턴스
오라클 데이터 베이스는 하나의 데이터 베이스의 하나이상의 인스턴스로 구성된다. 데어터베이스는 데이터를 저장하는 파일들의 모음이다.
#### 프로세스의 구조 
오라클 데이터 베이스의 프로세스는 배그라운드 프로세스와 서버프로세스로 구분된다. 백그라운드 프로세스는 인스턴스가 시작될때 서버프로세스는 사용자가 데이터베이스 서버에 접속할대 생성된다.서버프로세스는 클라이언트 프로세스와 통신할 수 있다.
#### 메모리 구조 
오라크 데이터 베이스의 메모리는 SGA와 PGA로 구분된다. SGA는 백그라운드 프로세느와 서버프로세스의 공유메모리 영역으로 인스턴스가 시작될때 할당되고 종료할때 해제한다. PGA는 서버프로세스의 독점 메모리 영역으로 서버프로세스가 생성될때 할당되고 종료될떄 해제된다.
#### 저장 구조
오라클 데이터베이스는 물리저장구조와 논리저장구조가 분리되어있다. 물리 구조를 변경할때 논리구조에 영향을 주지않는다.
##### 물리 저장 구조
물리 저장 구조는 파일이 저장됨 OS에서 확일할 수 있다.

|파일|설명|
|--|--|
|data file|세그먼트가 저장되는 파일  |
|control file|데이터베이스의 물리적인 저장요소에 대한 제어 파일  |
|online redo file  |데이터베이스의 변경 사항을 저장하는 파일 세트  |
|archived redo log  |archived redo log의 오프라인 사본  |
##### 논리 저장 구조
논리저장구조는 오라클 데이터베이스 내부에소 관리한다. 논리저장구조와 물리 저장 구조는 아래의 관계를 가진다. 세그먼트는 하나의 테이블 스페이스에 속하고 다수의 데이터 파일에 저장 될 수있다.

|단위|설명|
|--|--|
|블럭|데이터를 저장하는 가장작은 논리적 단위  |
|익스센트|논리적으로 연속된 data block의 집합|
|세그먼트|오브젝트에 할당된 extent의 집합|
|테이블스페이스|세그먼트를 포함하는 데이터베이스 저장단위  |
##### 네트워크 구조
리스너는 데이터베이스 서베에서 동작하는 프로그램으로 오라클 데이터베이스의 접속을 처리한다.
리스너 방식의 접속은 아래의 순서로 진행된다.

 1. 쿨라이언트 프로세스가 데이터베이스 서버에 접속을 요청
 2. 리스너가 클라이언트프로세스의 접속요청을 수락하고 서버프로세스를 생성
 3. 리스너가 서버프로세스와 클라이언트 프로세스를 연결한 후 다른접속요청을 대기

### SQL
SQL은 관계형 데이터베이스의 표준언어이다. 
#### 종류
|종류|구문|
|--|--|
|SELECT|SELECT|
|DML|INSERT, UPDATE, DELETE,MERGE|
|TCS|COMMIT,ROLLBACK,SAVEPOINT,SET TRANSACTION|
|DDL|CREATE, ALTER, DROP, TRUNCATE, COMMENT|
|DCL|GRANT,REVOKE|
|SCS|ALTER,SESSION,SET ROLE|
