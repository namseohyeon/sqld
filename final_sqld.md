## workbook C
1. 다이어그램
![image](https://user-images.githubusercontent.com/64974683/203309946-50c6a42a-c3af-4ca5-b445-3b953462f5ab.png)
- 실선은 부모 테이블의 기본키를 자식 테이블이 가지고 있으며, 이를 기본키로 사용하는 경우
- 점선은 부모 테이블의 기본키를 자식 테이블이 가지고 있지만, 이를 기본키로 사용하지 않는 

2. 테이블 구축
```
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
```

```
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
```

```
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
```

3. 데이터 입력

```
INSERT INTO Doctors
    values (070576, '피부과', '홍길동', 'M', '016-333-7263,'lJa@hanbh.com','전문의')
```

4. 데이터 변경 및 삭제
```
UPDATE Doctors
SET marjor_treat = "소아과"
where doc_name = "홍길동"
```
```
DELETE
FROM Nurses
WHERE nur_name = "김은영";
```
5. 데이터 검색
```
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
```

<!-- 연습문제 추가 예정 -->

## workbook A
1. 데이터 검색 select
```
select employee_id, first_name || last_name as "Name", salary, job_id,hire_date, manager_id  
from employees;
```
```
select last_name || ': 1 Year Salary = $' || salary*12 as "1 Year Salary"
from employees;
```
```
select distinct department_id, job_id
from employees;
```

2. 데이터 제한 및 정렬 where, order by
```
<!-- --샘플문제 -->
select first_name || ' ' || last_name, salary
from employees
where salary NOT BETWEEN 7000 AND 10000;

<!-- --A.2-1 -->
select last_name
from employees
where last_name LIKE '%e%' AND last_name LIKE '%o%'; 

<!-- --A.2-2 -->
select first_name ||' '|| last_name as "Name", salary, job_id, commission_pct
from employees
where commission_pct is NOT NULL
order by salary desc, commission_pct desc;
```

3. 단일 행 함수 및 변환 함수
: 단일 행 함수의 종류
- 문자 조작 함수 : 
    - upper() > 대문자 변환
    - lower() > 소문자 변환
    - replace() > 특정문자 교체 
    - substr() > 자르기
    - lpad() > 특정문자로 자릿수 채우기
    - concat() > 이어붙이기
- 숫자 관련 함수 : 
    - round() > 반올림
    - tranc([,m) > m이 있을때 입력한 부분까지 사용, 없으면 소수점 다 빼기
    - MOD() > 나머지 반환
    - POWER() > 제곱
- 날짜 관련 함수 :
    - SYSDATE() > 현재 시간
    - ADD_MONTHS() > 날짜에 월을 뺴거나 더하는 함수
    - LAST_DAY() > 입력한 날짜의 마지막 날짜를 가져오는 함수
    - MONTHS_BETWEEN() > 날짜와 날짜 사이의 개월 수 
- 변환 함수:
    - TO_CHAR() > 문자열로 변환
    - TO_DATE() > 날짜 포맷으로 변환
    - TO_NUMBER() > 수로 변환

```
<!-- --A.3-샘플 -->
select employee_id, first_name ||' '|| last_name as "Name", salary, salary*12.3 "incresed salary"
from employees
where department_id = 60;

<!-- --A.3-1 -->
select INITCAP(first_name) ||' '|| INITCAP(last_name)|| UPPER(job_id) "Employees JOBs"
from employees
where last_name LIKE '%s';

<!-- --A.3-2 -->
select first_name||' '||last_name "Name", salary*10 as "salary", NVL2(commission_pct,'Salary only','Commission')
from employees
order by salary desc;

<!-- --A.3-3 -->
select first_name||' '||last_name "Name", hire_date, to_char(hire_date, 'DAY')
from employees
order by to_char(hire_date,'d');
```

4. 집계된 데이터 보고: 집계함수
- 집계함수와 group by절
- 집계함수
    - sum()
    - AVG()
    - COUNT()
    - MAX()
    - MIN()
- group by절 : 테이블을 더 작게 그룹화함. 그룹을 나누는 기준으로 열, 계산식, 함수등을 사용할 수 있음 > group by 사용시 select절에 집계함수, group by 사용 속성 필요 
- having 절 : 집계 함수에 대한 조건을 지정할 수 있음
```
<!-- --A.4-샘플 -->
select count(distinct Manager_id)
from employees;

<!-- --A.4-1 -->
select department_id, TO_CHAR(avg(salary),'$999,999.000'), TO_CHAR(max(salary),'$999,999.000'), TO_CHAR(min(salary),'$999,999.000')
from employees
where department_id is NOT NULL
group by department_id
order by department_id asc;

<!-- --A.4-2 -->
select job_id, avg(salary)
from employees
where last_name Not LIKE 'CLERK'
group by job_id
having avg(salary) > 10000
order by avg(salary) desc;
```

5. 여러 테이블의 데이터 표시
- 조인
    - 동등조인 : 양쪽 테이블의 공통 칼럼을 기준으로 같은 행 연결
    - 비동등 조인: 양쪽 테이블의 공통 칼럼을 기준으로 동등이 아닌 조건으로 연결
    - 셀프 조인: 하나의 테이블에 공통된 칼럼이 두 개 이상 있는 경우의 동등조인
    - 외부 조인: 동등 조인에서 누락된 양쪽 테이블 정보를 출력하기 위한 조인으로 LEFT, RIGHT, FULL OUTER JOIN이 있음
    - 자연 조인: 양쪽 테이블에서 데이터 타입 및 컬럼명이 같은 값이 존재할 때 자동으로 동등 조인 수행
```
<!-- --A.5-1 -->
select d.department_name, count(e.employee_id)
from employees e, departments d
WHERE e.department_id = d.department_id
group by d.department_name
having count(e.employee_id) >= 5
order by 2 desc;

<!-- --A.5-2 pass

--A.5-3 -->
select s.first_name || ' ' || s.last_name || 'report to ' || UPPER(m.first_name) || ' ' || UPPER(m.last_name) "Employees vs Manager"
from employees s, employees m
where s.manager_id = m.employee_id(+);
```

6. 부속질의
- select, from , where, having절 사용 가능
- 종류
    - 단일 행 부속질의 : 부속질의의 결과 오직 하나의 행만 출력하여 전달
    - 다중 행 부속질의 : 부속질의의 결과 하나 이상의 행을 출력하여 전달
    - 다중 열 부속질의: 부속질의로 부터 두개이상의 컬럼을 비교 출력하여 전달 
    - 상관관계 부속 질의: 바깥 쪽 쿼리의 컬럼 중 하나가 안쪽 부속질의의 조건에 이용되는 처리방식 
    - from 절의 부속 질의: 뷰처럼 동작하여 인라인 뷰라고 불리기도 함. 하나의 테이블의 데이터가 많은 경우 from 절에 테이블 전체를 기술하면 효율이 떨어질 수 있음. 이 경우 필요한 행과 열만 선택하여 from절에 기술하면 오라클 서버가 최적화 단계에서 효율적인 검색을 할 수 있음 + 메인 절과 from절의 부속 질의를 비교하면 group by에 사용한 속성이 없어도 됨
    - 스칼라 부속질의: 하나의 행에서 하나의 열값만 반환하는 서브 쿼리를 말함

```
<!-- --A.6-샘플 -->
select first_name || ' ' || last_name as "name", job_id, salary
from employees
WHERE salary > (select salary from employees where last_name = 'Tucker');

<!-- --A.6-1 -->
select first_name || ' ' || last_name as "name", job_id, salary, hire_date
from employees
where (job_id,salary) IN (select job_id, min(salary) from employees group by job_id);

<!-- --A.6-2 -->
select e.first_name || ' ' || e.last_name as "name", e.job_id, e.salary, e.department_id, e.job_id
from employees e
where salary > (select avg(salary) 
                from employees 
                where department_id = e.department_id
                group by department_id);
                
<!-- --A.6-3 -->
select employee_id, first_name||' '||last_name "Name", job_id, hire_date
from employees
where department_id = (select department_id
                        from departments 
                        where location_id = (select location_id 
                            from locations 
                            where city LIKE'O%'));
                            
<!-- --A.6-4 -->
select first_name||' '||last_name "Name",
        job_id, salary, department_id, 
        (select ROUND(avg(salary)) from employees where e.department_id = department_id 
        group by department_id) as "Department Avg Salary"
from employees e
order by job_id desc;
```
