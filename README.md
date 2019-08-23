# Micorsoft SQL Server Coding Standard for CHUBB (작성중)

***

## 1. 기본규칙
* a. 들여쓰기 크기는 4이며 공백4개 기본으로 한다.
* b. AND 와 OR는 항상 괄호로 명시적으로 표현
* c. 여러 문장이 하나의 배치 파일에 작성될때 꼭 1칸의 빈 공백 라이을 준다
* d. 애스터리스크(*) 는 쓰지 않는다. 항상 명시적으로 컬럼 리스트를 지정
* e. MARS(쿼리 1개로 resultset(데이터셋) 을 여러개 받는것) 는 쓰지 않는다.
* f. 되도록 힌트를 쓰지 않는다. (최근에는 DBMS가 예전보다 똑똑해져서 일반적으로 사람보다 대부분 낫다)

## 2. 키워드
    a. 모든 SQL 키워드는 대문자 SELECT, FROM, WHERE, INSERT  
    b. 예약어를 변수명으로 사용하지 않는다.
    c. 모든 sql은 ;(세미콜론)으로 구분

## 3. 별칭
항상 AS 를 지정
 별칭은 단어들의 첫째 글짜로
   예) PRODUCT_BUY_CUSTOMER  ⇒ PBC

## 4. 주석
#### a. 한줄 주석
-- 대시 두번이면 같은 줄의 이후의 내용은 주석처리됨

#### b. 복수줄 주석
대량의 내용을 주석으로 하고 싶을때
```sql
/**************************************************************
-- Author 	: 박성출
-- Create date: 2012-09-08
-- Description: 초기 작성
 
DATE         	Developer        Change
----------   	---------------- -------------------------------
2012-09-08      박성출         	  처음 작성
**************************************************************/
```


## 5. SELECT
기본 스타일은 다음과 같습니다.
```sql
SELECT MEMBER_NO, MEMBER_ID, MEMBER_NAME, BASIC_SYSTEM_NO, CREATE_DTM
	, MEMBER_ID, MEMBER_NAME
FROM TB_MEMBER
WHERE MEMBER_GROUP_CODE = 'A'
```
    SELECT, FROM, WHERE 절은 다음줄에 배치  
    SELECT절에 한줄의 컬럼 갯수는 5개까지만 하고 6개부터 줄바꿈  

5.1 SELECT 절에는 가급적 사용자 정의 함수를 사용하지 않는다. 성능 이슈 존재.  
    해당 로직을 함수 밖으로 꺼낸다.  
5.2  컬럼과 컬럼사이의 콤마 규칙은 다음과 같다   
```sql   
    -- 한줄일때
    SELECT PROD_NO, CUST_NAME

    -- 여러줄일때
    SELECT PROD_NO
        , CUST_NAME
```

5.3 SELECT, FROM, WHERE, GROUP BY 는 각각 다른 줄에  


5.4 WHERE 조건은 다른줄에
```sql    
SELECT *
FROM TABLE1
WHERE PROD_NO = 1
	AND CUST_NO = 55552
	AND REGION = ‘서울’
```

5.6 SELECT 구문의 가로길이는 컬럼 5개  
```sql
    SELECT PROD_NO, CUST_NAME, CUST_NO, CUST_ADDR, CUST_TELNO
        , CUST_ALIAS_NAME
```        
5.7 변수에 값 할당시 SET을 쓴다. 단 테이블에서 여러 값을 한꺼번에 변수에 넣을때만 SELECT
```sql
    -- 권장방식
    DECLARE @TEMP_NAME VARCHAR(50)
    SET @TEMP_NAME = '홍길동'
    
    -- 비권장
    DECLARE @TEMP_NAME VARCHAR(50)
    SELECT @TEMP_NAME = '홍길동'

    -- 테이블에서 값을 변수에 할당할때만 SELECT 사용
    DECLARE @TEMP_NAME VARCHAR(50)

    SELECT @TEMP_NAME = 
    FROM MEMBER
    WHERE CUST_NO = 5524
```       


## 6. UPDATE
기본 스타일은 다음과 같습니다.

```SQL
UPDATE T MEMBER
SET MEMBER_NAME = '홍길동'
    , MEMBER_GROUP_CODE = 'C'
WHERE MEMBER_NO = 5
    AND MEMBER_GROUP_CODE = 'A'
```

#### a. UPDATE, SET, WHERE 는 다른줄에 배치
#### b. SET절과 WHERE절에서 컬럼들 다른줄에 배치


## 7. INSERT
    컬럼 리스트를 항상 명시
```SQL    
    -- 나쁜 예
    INSERT INTO TABLE1 VALUES (1, getdate();
    
    -- 좋은 예
    INSERT INTO TABLE1 (NO, TODAY_DATE) VALUES (1, getdate());
```

## 8. DELETE

## 9. 에러처리
* GOTO 논리를 사용하지 않는다

* null값 비교는 무조건 IS NULL 또는 IS NOT NULL 

* 오류처리는 TRY CATCH 사용
```SQL
BEGIN TRY
    TSQL 쿼리
END TRY
BEGIN CATCH
    에러처리
END CATCH
```

## 10. 조인
조인은 ANSI 조인을 사용
```SQL
SELECT PROD_NO
FROM TABLE1 AS T1
    JOIN TABLE2 AS T2         	ON T1.TNO = T2.TNO
    JOIN CUST_BUY_LIST AS CBL 	ON T2.CUST_NO = CBL.CUST_NO
```

#### a. INNER JOIN 은 INNER를 생략한 JOIN
#### b. LEFT OUTER JOIN, RIGHT OUTER JOIN  ⇒ LEFT JOIN, RIGHT JOIN, FULL JOIN
#### c. ON 절은 탭으로 1번
#### d. JOIN은 1탭 들여쓰기한다.
```SQL
    SELECT PROD_NO
    FROM TABLE1 AS T1
        JOIN TABLE2 AS T2		ON T1.TNO = T2.TNO
```

#### e.  여러 테이블 조인시 항상 최상위 테이블부터
```SQL
    SELECT C.NAME
    FROM CUST AS C
        JOIN CUST_BUY_LIST AS CBL	ON 
```

## 11. 피해야 할 점
텍스트 형태의 데이터 타입 CHAR, NCHAR, VARCHAR, NVARCHAR 에는 항상 크기를 지정
```SQL
/* Correct */
 DECLARE @myGoodVarchareVariable  VARCHAR(50);
 DECLARE @myGoodNVarchareVariable NVARCHAR(90);
 DECLARE @myGoodCharVariable      CHAR(7);
 DECLARE @myGoodNCharVariable     NCHAR(10);
 
 /* Not correct */
 DECLARE @myBadVarcharVariable  VARCHAR;
 DECLARE @myBadNVarcharVariable NVARCHAR;
 DECLARE @myBadCharVariable     CHAR;
 DECLARE @myBadNCharVariable    NCHAR;
```


## 12. 권장
### a. 일부 로우만 가져오기
SELECT의 결과셋을 제한할때 TOP을 사용
```SQL
SELECT TOP 5 PROJECT_NO, PROJECT_NAME
FROM T_PROJECT


SELECT TOP 50 PERCENT PROJECT_NO, PROJECT_NAME  -- 결과중 절반만 가져오기
FROM T_PROJECT
```

#### b. 서브쿼리
```SQL
SELECT MEMBER_NO, MEMBER_ID
FROM TB_MEMBER
WHERE MEMBER_NO = (SELECT TOP 1 MEMBER_NO
                                         FROM TB_MEMBER
                                         WHERE MEMBER_GROUP_CODE = 'A')


SELECT M.MEMBER_NO, M.MEMBER_ID, M.MEMBER_NAME
FROM TB_MEMBER M
    JOIN
    (
        SELECT MEMBER_NO, MEMBER_NAME, MEMBER_GROUP_CODE
        FROM TB_MEMBER
        WHERE MEMBER_GROUP_CODE = 'A'
    ) A    ON M.MEMBER_NO = A.MEMBER_NO
WHERE M.MEMBER_GROUP_CODE NOT IN ('A')
```

#### c. @@IDENTITY 사용 금지
@@IDENTITY 대신 SCOPE_IDENTITY() 사용


#### d. NOLOCK 힌트 사용 금지
Azure SQL Database           : read commited snapshot isolation가 기본  
Azure SQL Datawarehouse : read uncommited가 default 


- 변수 선언은
DECLARE @MEM_NAME VARCHAR(500)  = '박성출'       --  변수 선언

데이터 존재 파악
- 데이터 존재 여부 파악을 위해 COUNT(*) 또는 SELECT * FROM 을 사용하는 대신 EXSITS/TOP 구문 사용
-- 예제 1. 첫번째 한건만 확인
IF (EXISTS(SELECT 1 FROM DBO.TB_CUSTID WHERE CUSTNAME LIKE '박%'))
 
-- 예제 2. 첫번째 한건의 컬럼만 확인
IF (EXISTS(SELECT TOP 1 CUSTID FROM DBO.TB_CUSTID WHERE CUSTNAME LIKE '박%'))
 
 
-- 나쁜 예.
IF (SELECT COUNT(*) FROM DBO.TB_CUSTID WHERE CUSTNAME LIKE ‘박%’)) -- 전체 ROW 를 모두 COUNT
