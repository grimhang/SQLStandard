# Micorsoft SQL Server Coding Standard
Microsoft SQL Server SQL Code Standard

## 0. 기본규칙

## 1. 키워드

## 2. 별칭

## 3. 주석

## 4. SELECT

## 5. UPDATE
기본 스타일은 다음과 같습니다.

```SQL
UPDATE T MEMBER
SET MEMBER_NAME = '홍길동'
    MEMBER_GROUP_CODE = 'C'
WHERE MEMBER_NO = 5
    AND MEMBER_GROUP_CODE = 'A'
```

#### a. UPDATE, SET, WHERE 는 다른줄에 배치
#### b. SET절과 WHERE절에서 업데이트할 컬럼들은 다른줄에 배치


## 6. INSERT

## 7. DELETE

## 에러처리
- GOTO 논리를 사용하지 않는다

- null값 비교는 무조건 IS NULL 또는 IS NOT NULL 

- 오류처리는 TRY CATCH 사용
```SQL
BEGIN TRY
    TSQL 쿼리
END TRY
BEGIN CATCH
    에러처리
END CATCH
'''

## 10. 조인

##피해야 할 점
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


## 권장
#### A. 일부 로우만 가져오기
```SQL
SELECT의 결과셋을 제한할때 TOP을 사용
SELECT TOP 5 PROJECT_NO, PROJECT_NAME
FROM T_PROJECT


SELECT TOP 50 PERCENT PROJECT_NO, PROJECT_NAME
FROM T_PROJECT
```

#### B. 서브쿼리
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

-- @@IDENTITY 사용 금지
#### C. @@IDENTITY 사용 금지
@@IDENTITY 대신 SCOPE_IDENTITY() 사용


#### D. NOLOCK 힌트 사용 금지
Azure SQL Database           : read commited snapshot isolation가 기본
Azure SQL Datawarehouse : read uncommited가 default 


