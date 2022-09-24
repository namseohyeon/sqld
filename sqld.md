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
