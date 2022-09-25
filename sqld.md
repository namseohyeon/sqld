### DDL
- DESCRIBE 테이블명 / 생성된 테이블 구조 확인
- exec sp_help 테이블명 / 생선된 테이블 구조 확인
- create table 뉴테이블 as select * from 테이블명 / select 문장을 통한 테이블 생성
- alter table 
    - add (추가할 칼럼명) / 새롭게 칼럼 추가하기(마지막 칼럼이 됨)
    - drop column 바꿀 칼럼명
    - modify (칼럼명 데이터 유형 [DEFAULT 식] [NOT NULL], ...)
    - rename column 변경해야할 칼럼명 to 새로운 칼럼명
    - drop constraint 제약조건명 / 제약조건 삭제
    - add constraint 제약조건명 (칼럼명) / 제약조건 추가
- rename 변경전 테이블명 to 변경후 테이블명
- drop table 테이블명 
- truncate table 테이블명 / 테이블 자체 삭제 아니고, 테이블에 있던 모든 행들이 제거(테이블 완전 삭제하려면 drop)
    - 시스템 부하 적음
    - rollback 안됨 > 정상적인 복구 불가
- 데이터유형 character(s), varchar(s), numeric, datetime

### DML 
- INSERT INTO 테이블명 (COLUMN_LIST) VALUES (COLUMN_LIST에 넣을 VALUE_LIST)
- INSERT INTO 테이블명 VALUES (전체 COLUMN에 넣을 VALUE_LIST)
- INSERT INTO 테이블명 (칼럼) VALUES (값) 
- UPDATE 테이블명 SET 수정해야 할 칼럼명 = 수정되기 원하는 새로운 값
- DELETE FROM 테이블명
- SELECT [ALL/DISTINCT ] 칼럼명, .. FROM 테이블명 / ALL 모든 행, DISTINCT 중복제거
- SELECT 칼럼 AS 칼럼이름 
    - AS 생략가능
    - 이중 인용부호는 공백, 특수문자 포함 시 대소문자 구분이 필요한 경우 사용
- SELECT 산술 연산자(+,-,/,*) 사용가능
- SELECT 칼럼 || '뫄뫄' FROM 테이블명 // SELECT 합성연산자, 문자와 문자연결 
- DUAL 테이블
    - 사용자 테이블이 필요없는 SQL문장의 경우에는 필수적으로 DAUL이라는 테이블을 FROM절에 지정
- where
    - IN / WHERE (칼럼명) IN ((값),..)
    - LIKE / WHERE 칼럼명 '_장%' # _: 1개인 단일문자, %: 0개 이상의 어떤 문자를 의미 
    - IS NULL / WHERE 칼럼 IS NULL 
    - ROWNUM <= 1 #원하는 만큼의 행가져오기, ORACLE사용
    - SELECT TOP(N) 컬럼명 FROM 테이블명 #반환할 향의 수를 지정하는 숫자, SQL SERVER 사용
    - 비교연산자, SQL연산자, 논리연산자, 부정연산자 사용가능


### TCL(트랜잭션 컨트롤)
- commit : 변경된 데이터를 데베에 영구적으로 반영
- rollback : 변경전 데이터로 복귀
- savepoint : 데이터 변경을 사전에 지정한 저장점까지 롤백

### 함수
- 문자형 함수
    - LOWER(문자열) : 문자열을 소문자로
    - UPPER(문자열) : 문자열은 대문자로
    - ASCII(문자) : 문자나 숫자를 ASCLL 코드번호로 바꿈
    - CHR/CHAR(ASCII번호) : 문자나 숫자로 바꿔줌
    - CONCAT(문1,문2) : 문자열연결, ||와 동일
    - SUBSTR/SUBSTRING(문자열,m,n) : m위치에서 n개의 문자 길에 해당하는 문자를 돌려줌
    - LENGTH/LEN(문자열) : 문자열의 개수
    - TRIM
        - LTRIM(문자열, m) : 문자열의 첫문자부터 확인해서 지정 문자가 나타나면 해당 문자 제거
        - RTRIM(문자열, m) : 마지막부터 확인
        - 지정문자가 없을 시 공백값이 디폴트
        - TRIM([leading|trailing|both ]) : 머리말, 꼬리말 또는 양쪽에 있는 지정 문자 제거
            - both가 디폴트
- 숫자형 함수
    - ABS(숫자) : 숫자의 절댓값
    - SIGN(숫자) : 숫자가 양수인지, 음수인지 0인지 구별
    - MOD(숫자1,숫자2) : 숫자1을 숫자2로 나누어 나머지 값을 리턴
    - CEIL/CEILNG(숫자) : 숫자보다 크거나 같은 최소정수를 리턴
    - FLOOR(숫자) : 숫자보다 작거나 같은 최대 정수를 리턴
    - ROUND(숫자,[m ]) : m자리에서 반올림하여 리턴 + 디폴트 0
    - TRUNC(숫자, [m ]) : 숫자를 소수 m자리에서 자름 + 디폴트 0
    - EXP() : 숫자의 지수
    - POWER() : 거듭제곱
    - SQRT() : 제곱근
    - LOG() : 자연로그 값
- 날짜형 함수
    - SELECT SYSDATE FROM DUAL /GETDATE() #현재 날짜 시각 출력
    - EXTRACT('YEAR'|'MONTH'|'DAY' from d)/DATEPART('YEAR'|'MONTH'|'DAY',d) 년/월/일 데이터 출력 시간/분/초도 가능
    - TO_NUMBER(TO_CHAR(d,'YYYY'))/YEAR(d) #날짜데이터에서 년데이터 출력
    - TO_NUMBER(TO_CHAR(d,'MM'))/MONTH(d) #월 데이터 출력
    - TO_NUMBER(TO_CHAR(d,'DD'))/DAY(d) #일 데이터 출력
    - 날짜 계산 가능
- 명시적 함수
    - SELECT TO_CHAR(숫자|날짜 [, FORMAT]) FROM DUAL #FORMAT형태로 문자열타입으로 변환
    - TO_NUMBER(문) #숫자로 변경
    - TO_DATE(문, [, FORMAT]) #FORMAT형태로 날짜 타입으로 변환
- CASE 표현
- DECODE(표현식, 기준값1,값1) #oracle에서 사용, 표현식 값이 기준값1이면 값1 출력
```
#simple_case_expression
# EQUL(=)만 사용
SELECT LOC,
    CASE LOC
    WHEN 'NEW YORK' THEN 'EAST'
    WHEN 'BOSTON' THEN 'EAST'
    WHEN 'CHICAGO' THEN 'CENTER'
    ELSE 'ETC'
    END as AREA
FROM DEPT
```
```
#searched_case_expression
# EQUL(=) 추가로 (>,>=,<,<=)사용가능
SELECT ENAME
    CASE 
    WHEN m >= 3000 THEN 'EAST'
    WHEN m >= 1000 THEN 'EAST'
    ELSE 'ETC'
    END as AREA
FROM DEPT
```
```
#case중첩
SELECT ENAME,SAL
    CASE WHEN SAL >= 2000
    THEN 1000
    ELSE (CASE WHEN SAL >= 1000)
            TEHN 500
            ELSE 0
        END)
    END as BOUNS
FROM DEPT
```
- NULL 관련 함수
    - <b>NVL/ISNULL</b>(표현식1, 표현식2) #표현식1의 결과값이 NULL이면 표현값2의 값 출력(1,2 타입이 같아야함)
    - NULLIF(표현식, 표현식2) #같으면 NULL, 아니면 표현식1 리턴
    - COALESE(표현식1, 표현식2) 표현식에서 NULL이 아닌 최초의 표현식을 나타냄. 모든 표현식이 NULL이면 NULL을 리턴
- 집계 함수
    - COUNT(표현식)
    - SUM([DISINCT |ALL]표현식) #합출력
    - AVG([DISINCT |ALL]표현식) #평균값 출력
    - MAX([DISINCT |ALL]표현식) #최댓값
    - MIN([DISINCT |ALL]표현식) #최소값
    - STDDEV([DISINCT |ALL]표현식) #표현식의 표준편차 출력
    - VARIAN([DISINCT |ALL]표현식) #표현식의 분산 출력
- GROUP BY
    - FROM, WHERE절 뒤에 오며 GROUP BY절은 행들을 소그룹화함
    - 집합별 통계정보의 기준을 명시
    - NULL 값 행 제외 후 수행
    - GROUP BY 칼럼명
    - ALIAS명 사용 불가
- HAVING
    - GROUP BY절에 의해 만들어진 소그룹에 대한 조건 적용
    - 집합에 대한 조건을 두어 조건 만족시 내용 출력
```
GROUP BY 칼럼명
HAVING AVG(HEIGHT) >= 180
```
- ORDER BY
    - 특정 칼럼 기준으로 정렬
    - 기본적으로 오름차순
    - NULL값이 가장 큰 값
    - FROM, WHERE 뒤쪽에 위치
    - ASC #오름차순 정렬, 기본값으로 생략가능
    - DESC #내림차순 정렬
    - [ORDER BY 칼럼&표현식 [ASC나 DESC]]  
    - 칼럼명, ALIAS명, 칼럼 순서 혼용가능

##### SELECT 문장 실행 순서
- from절에서 ALIAS 사용 시 select에서 반드시 ALIAS 사용
- 작성 순서
```
SELECT 칼럼명 #5 
FROM 테이블명 #1
WHERE 조건식 #2
GROUP BY 칼럼이나 표현식 #3
HAVING 그룹조건식 #4
ORDER BY 칼럼이나 표현식 #6
```
- 작동 순서
1. 발췌 대상 테이블 참조(FROM)
2. 발췌 대상 데이터가 아닌 것을 제거(WHERE)
3. 행들을 소그웁화한다(GROUP BY)
4. 그룹핑 된 값의 조건에 맞는 것 출력(HAVING)
5. 데이터 값을/출력 계산(SELECT)
6. 데이터 정렬(ORDER BY)

### JOIN
- EQUI JOIN #두 개의 테이블 간에 칼럼 값들이 서로 정확하게 일치하는 경우 사용됨
    - WHERE 테이블1.칼럼명1 = 테이블2.칼럼명2
    - where 절에 조건 표시
    - END 사용가능
- NON EQUI JOIN # 값 일치 안할때
    - between, >,>=,<,<= 사용