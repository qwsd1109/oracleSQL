# 불친절한 SQL 프로그래밍
## 샘플 데이터 생성




    CREATE TABLE 부서(
	    부서번호 NUMBER NOT NULL,
	    부서명 VARCHAR2(10) NOT NULL,
	    상위부서번호 NUMBER,
	    CONSTRAINTS 부서_PK PRIMARY KEY (부서번호),
	    CONSTRAINTS 부서_F1 FOREIGN KEY (상위부서번호) REFERENCES 부서 (부서번호));
	    ----------------- docker 오라클11g 한글지원 방법을 못찾음-------
	CREATE TABLE Department(
	    DepartmentNo NUMBER NOT NULL,
	    DepartmentNa VARCHAR2(10) NOT NULL,
	    TopDepartmentNo NUMBER,
	    CONSTRAINTS Department_PK PRIMARY KEY (DepartmentNo),
	    CONSTRAINTS Department_F1 FOREIGN KEY (TopDepartmentNo) REFERENCES Department (DepartmentNo));
	
	CREATE TABLE Employee(
	    EmployeeNo NUMBER NOT NULL,
	    EmployeeNa VARCHAR2(10) NOT NULL,
	    Salary NUMBER NOT NULL,
	    AffDepartmentNo NUMBER NOT NULL,
	    CONSTRAINTS Employee_PK PRIMARY KEY (EmployeeNo),
	    CONSTRAINTS Employee_F1 FOREIGN KEY (AffDepartmentNo) REFERENCES Department (DepartmentNo));
	    
	CREATE TABLE CooFirm(
	    CompanyNo NUMBER NOT NULL,
	    CompanyNa VARCHAR2(10) NOT NULL,
	    CONSTRAINTS CooFirm_PK PRIMARY KEY (CompanyNo));
	
	CREATE TABLE Client(
	    ClientNo NUMBER NOT NULL,
	    ClientNa VARCHAR2(10) NOT NULL,
	    ClientTy VARCHAR2(10) NOT NULL,
	    CONSTRAINTS Client_PK PRIMARY KEY (ClientNo));
	
	CREATE TABLE IndClient(
	    ClientNo NUMBER NOT NULL,
	    RegistrationNum VARCHAR2(13) NOT NULL,
	    CONSTRAINTS IndClient_PK PRIMARY KEY (ClientNo),
	    CONSTRAINTS IndClient_U1 UNIQUE (RegistrationNum),
	    CONSTRAINTS IndClient_F1 FOREIGN KEY (ClientNo) REFERENCES Client (ClientNo));
	
	CREATE TABLE CorClient(
	    ClientNo NUMBER NOT NULL,
	    CorporationNum VARCHAR2(13) NOT NULL,
	    CONSTRAINTS CorClient_PK PRIMARY KEY (ClientNo),
	    CONSTRAINTS CorClient_U1 UNIQUE (CorporationNum),
	    CONSTRAINTS CorClient_F1 FOREIGN KEY (ClientNo) REFERENCES Client (ClientNo));
	
	CREATE TABLE Product(
	    ProductCode VARCHAR2(10) NOT NULL,
	    ProductNa VARCHAR2(10) NOT NULL,
	    PlanNum NUMBER ,
	    BuyNum NUMBER ,
	    BuyCompany NUMBER ,
	    RelatedDepart VARCHAR2(50) NOT NULL,
	    CONSTRAINTS Product_PK PRIMARY KEY (ProductCode));
	    
	CREATE TABLE ProductPrice(
	    ProductCode VARCHAR2(10) NOT NULL,
	    StartDate DATE NOT NULL,
	    EndDate DATE NOT NULL ,
	    ProductPrice NUMBER NOT NULL,
	    CONSTRAINTS ProductPrice_PK PRIMARY KEY (ProductCode,StartDate),
	    CONSTRAINTS ProductPrice_F1 FOREIGN KEY (ProductCode) REFERENCES Product (ProductCode));
	    
	CREATE TABLE DiscountCoupon(
	    CouponCode VARCHAR2(10) NOT NULL,
	    PublicationDate DATE NOT NULL,
	    ExpirationDate DATE NOT NULL ,
	    DiscountRate NUMBER NOT NULL,
	    TargetProductCode VARCHAR2(10) NOT NULL,
	    CONSTRAINTS DiscountCoupon_PK PRIMARY KEY (CouponCode),
	    CONSTRAINTS DiscountCoupon_F1 FOREIGN KEY (TargetProductCode) REFERENCES Product (ProductCode));
	
	CREATE TABLE Order_(
	    OrderNumber NUMBER NOT NULL,
	    OrderDate DATE NOT NULL,
	    OrderCustomerNumber NUMBER NOT NULL ,
	    CONSTRAINTS Order_PK PRIMARY KEY (OrderNumber),
	    CONSTRAINTS Order_F1 FOREIGN KEY (OrderCustomerNumber) REFERENCES IndClient (ClientNo));
	    
	CREATE TABLE OrderDetails(
	    OrderNumber NUMBER NOT NULL,
	    ProductCode VARCHAR2(10) NOT NULL,
	    OrderQuantity NUMBER NOT NULL ,
	    CONSTRAINTS OrderDetails_PK PRIMARY KEY (OrderNumber,ProductCode),
	    CONSTRAINTS OrderDetails_F1 FOREIGN KEY (OrderNumber) REFERENCES Order_ (OrderNumber),
	    CONSTRAINTS OrderDetails_F2 FOREIGN KEY (ProductCode) REFERENCES Product (ProductCode));
	    
	CREATE TABLE SalesStatistics(
	    ProductCode VARCHAR2(10) NOT NULL,
	    BaseDate VARCHAR2(6) NOT NULL,
	    SalesQuantity NUMBER NOT NULL ,
	    SalesAmount NUMBER NOT NULL,
	    CONSTRAINTS SalesStatistics_PK PRIMARY KEY (ProductCode,BaseDate));
	
	INSERT INTO Department VALUES (1,'Depart1',NULL);
	INSERT INTO Department VALUES (2,'Depart2',1);
	INSERT INTO Department VALUES (3,'Depart3',2);
	
	INSERT INTO Employee VALUES (1,'Emplo1',300,1);
	INSERT INTO Employee VALUES (2,'Emplo2',200,2);
	INSERT INTO Employee VALUES (3,'Emplo3',100,3);
	--------------------------------------------------------
	INSERT INTO CooFirm VALUES (1, 'CooFirm1');
	INSERT INTO CooFirm VALUES (2, 'CooFirm2'); 
	INSERT INTO CooFirm VALUES (3, 'CooFirm3');
	
	INSERT INTO Client VALUES (1, 'Client1', 'P');
	INSERT INTO Client VALUES (2, 'Client2', 'P');
	INSERT INTO Client VALUES (3, 'Client3', 'C');
	
	INSERT INTO IndClient VALUES (1, '0101013000001');
	INSERT INTO IndClient VALUES (2, '0101013000002');
	
	INSERT INTO  CorClient VALUES (3, '1101110000001');
	
	INSERT INTO Product VALUES ('A','ProductA',2,3,NULL,'1,2,3');
	INSERT INTO Product VALUES ('B','ProductB',2,2,NULL,'1,2');
	INSERT INTO Product VALUES ('C','ProductC',2,NULL,1,'1');
	
	INSERT INTO ProductPrice VALUES ('A',DATE '2011-01-01',DATE '2011-12-31',100);
	INSERT INTO ProductPrice VALUES ('A',DATE '2012-01-01',DATE '9999-12-31',200);
	INSERT INTO ProductPrice VALUES ('B',DATE '2011-01-01',DATE '2011-06-30',300);
	INSERT INTO ProductPrice VALUES ('B',DATE '2011-07-01',DATE '9999-12-31',400);
	INSERT INTO ProductPrice VALUES ('C',DATE '2011-02-01',DATE '9999-12-31',500);
	
	INSERT INTO DiscountCoupon VALUES ('X',DATE '2011-01-01',DATE '2011-06-30',0.1,'A');
	INSERT INTO DiscountCoupon VALUES ('Y',DATE '2011-07-01',DATE '9999-12-31',0.2,'A');
	
	INSERT INTO Order_ VALUES (1,DATE '2011-01-01',1 );
	INSERT INTO Order_ VALUES (2,DATE '2011-07-01',1 );
	INSERT INTO Order_ VALUES (3,DATE '2011-01-01',2 );
	INSERT INTO Order_ VALUES (4,DATE '2012-07-01',2 );
	
	INSERT INTO OrderDetails VALUES (1, 'A', 10);
	INSERT INTO OrderDetails VALUES (2, 'A', 20);
	INSERT INTO OrderDetails VALUES (2, 'B', 30);
	INSERT INTO OrderDetails VALUES (3, 'A', 100);
	INSERT INTO OrderDetails VALUES (4, 'A', 200);
	INSERT INTO OrderDetails VALUES (4, 'B', 300);
	
	INSERT INTO  SalesStatistics VALUES ('A','201101',110,9900);
	INSERT INTO  SalesStatistics VALUES ('A','201107',20,1600);
	INSERT INTO  SalesStatistics VALUES ('A','201207',200,32000);
	INSERT INTO  SalesStatistics VALUES ('B','201107',30,12000);
	INSERT INTO  SalesStatistics VALUES ('B','201207',300,120000);
