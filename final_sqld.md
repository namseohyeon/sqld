1. 다이어그램
설명 추가

2. 테이블 구축
'''
create table Doctors(
    doc_id number(10) NOT NULL,
    major_treat varchar2(25) NOT NULL,
    doc_name varchar2(20) NOT NULL,
    doc_gen char(1) NOT NULL,
    doc_phone varchar2(15)
    doc_email varchar2(50) UNIQUE,
    doc_position varchar2(20) NOT NULL
)

ALTER TABLE Doctors
    ADD CONSTRAINT doc_id PRIMARY KEY(doc_id);
'''

'''
create table nurses(
    nur_id number(10) NOT NULL,
    major_id varchar2(25) NOT NULL,
    nur_name VARCHAR2(20) NOT NULL,
    nur_gen char(1) NOT NULL,
    nur_phone varchar2(15),
    nur_email varchar2(50) UNIQUE,
    nur_position varcher2(20) NOT NULL
)
alter table nurses
    ADD CONSTRAINT nur_id PRIMARY KEY (nur_id);
'''
'''
create table treatments(
    treat_id number(15) NOT NULL,
    pat_id number(10) NOT NULL,
    doc_id number(10) NOT NULL,
    treat_contents VARCHAR2(1000) NOT NULL,
    treat_date date NOT NULL 
)

ALTER table treatments
    ADD CONSTRINT treat_pat_coc_id_pk PRIMARY KEY (treat_id, pat_id, doc_id)
ALTER table treatments
    ADD (CONSTRINT R_5 FOREIGN KEY (pat_id) REFERENCES Patients (pat_id));
Alter table treatments
    ADD (CONSTRINT R_6 FOREIGN KEY (doc_id) REFERENCES Doctors (doc_id));
'''

3. 데이터 입력
'''
INSERT INTO Doctors
    values (070576, '피부과', '홍길동', 'M', '016-333-7263,'lJa@hanbh.com','전문의')
'''

4. 데이터 변경 및 삭제
'''
UPDATE Doctors
SET marjor_treat = "소아과"
where doc_name = "홍길동"
'''
'''
DELETE
FROM Nurses
WHERE nur_name = "김은영";
'''

5. 데이터 검색
'''
<!-- 담당 진료과목이 소아과인 의사에 대한 정보 출력 -->
select * from doctors where marjor_treat= "소아과"

<!-- 홍길동 의사에게 진료를 받은 환자에 대한 모든 정보 출력 -->
select * 
from patients, doctors 
where patients.doc_id = doctors.doc_id
AND doctors.doc_name = '홍길동';

SELECT p.*, d.doc_name
FROM Doctors d JOIN Patients p ON (d.doc_id = p.doc_id)
WHERE d.doc_name = '홍길동';

<!-- 진료날짜가 2013년 12월인 환자에 대한 모든 정보를 오름차순 정렬하여 출력하시오 -->
select p.*, t.treat_date
from patients p,treatments t
where p.pat_id = t.pat_id
AND t.treat_date between '2013-12-01' AND '2013-12-31';

SELECT p.*, t.treat_date
FROM Treatments t JOIN Patients p ON (t.pat_id = p.pat_id)
WHERE t.treat_date between '2013-12-01' and '2013-12-31'; 
ORDER BY t.treat_date ASC;

<!-- 간호사 ID가 05로 시작하는 모든 간호사의 정보 출력 -->
select *
from nurses
where nur_id LIKE '5%';
'''

<!-- 연습문제 추가 예정 -->