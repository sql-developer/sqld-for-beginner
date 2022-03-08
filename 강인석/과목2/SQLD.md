# 데이터 조작언어
## DML(Data Manipilayion Language) 데이터 조작어
### 1. SELECT :데이터 조회


### 2. DELETE :데이터 삭제
~~~
DELETE FROM 테이블 
wHERE 컬럼 = 삭제할 데이터
~~~
주의 : 
1. 해당 컬럼이 ON DELETE CASCADE 로 설정되어 있으면, 연관된 컬럼 모두 삭제됨.
    실제로는 데이터 삭제x
    삭제여부 컬럼을 만듬 ex) 컬럼이름 : 회원탈퇴 여부
                             데이터 : 'y' or 'n'

### 3. UPDATE :데이터 수정
~~~
update 테이블
set 
컬럼이름 =  데이터
where 컬럼이름 = 바꿀위치
~~~

### 4. INSERT :데이터 생성
~~~
 작성법 1.
    ex)INSERT INTO 테이블 이름(컬럼이름 [ex)'name',sal,job])
       VALUES(컬럼에해당하는데이터 [ex)'홍길동',3000,'manager']) 
    주의 : 테이블에 NULLABLE 
        1. 테이블에 job컬럼이 not null이면, insert할떄 무저건 데이터를 넣어야 함. 
        2. 테이블에 기본키가 auto increment 가 아니라면, 기본키 데이터를 넣어야 함.
        3. commit 을해야  최종 insert가 됨. (디비버 같은 프로그램은 auto commit 으로 설정되어 있음.)
    작성법 2.
    해당 테이블에 데이터를 모두 넣으면, 테이블 뒤 괄호 생략
    INSERT INTI 테이블
    VALUES(컬럼에해당하는데이터 [ex)'홍길동',3000,'manager',...])
~~~

## DDL(data Definition Language) 데이터 정의 언어
## DDL (auto commit)
### 1.CREATE => 테이블 생성!
    CREATE TABLE <table_name> : table 생성
~~~    
    문법:
    CREATE TABLE 테이블이름 (컬럼이름 데이터타입 조건)
    ex) CREATE TABLE student(id INT(11) NOT NULL,name VARCHAR(20) NOT NULL, height INT(5),age INT(5) DEFAULT 0, create_at DATETIME DEFAULT CURRENT_TIMESTAMP);
    -- pk키를 만들면서 테이블만들기 (스키마)
    CREATE TABLE student(id INT(11) NOT null auto_increment,
    name VARCHAR(20) NOT null , height INT(5),
    age INT(5) DEFAULT 0, 
    create_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    ***CONSTRAINT student_id_pk primary key(id)
);
~~~
### 2. DROP
DROP TABLE <table_name>   : table 삭제
~~~
DROP TABLE 테이블이름
~~~

### 3. ALTER
ALTER TABLE <table_name>  : table 수정 (테이블 이름 변경할떄씀)
~~~
ALTER TABLE [테이블명] MODIFY [컬럼명] [타입]
ALTER TABLE ex_table MODIFY COLUMN sFifth VARCHAR(55);
~~~
### RENAME 
RENAME COLUMN 변경점칼럼 to NEW칼럼명

## DCL (data control Language) 데이터 제어언어
### 1. GRANT  : 특정 사용자에게 권한 부여
### 2. REBOKE : 특정 사용자 권한 회수

## TCL  논리적인 작업의 단위를 묶어서 DML에 의해 조작된 결과를 작업단위 별로 제어하는 명령어
### 1. COMMIT (DML은 커밋 미포함,DDL은 커밋 포함)
    COMMIT 문은 관계형 데이터베이스 관리 시스템(RDBMS)에서 트랜잭션을 종료하고 다른 사용자에게 변경된 모든 사항을 보이도록 만드는 문이다. 
    일반적으로 트랜잭션 종료시 해당 업데이트를 확정한다는 의미에서 "커밋"이라고 사용한다.

### 2. ROLLBACK : 실수로 DELETE 해도 ROLLBACK하면 최신 커밋상태로 돌아감
~~~
예졔)
UPDATE A SET VAL = 200 WHERE ID = '001';
CREATE TABLE B ( ID CHAR(3) PRIMARY KEY);
ROLLBACK;
~~~


## PRIMARY KEY(기본키)
해당 테이블의 식별자역할
테이블에 하나만 설정할 수 있는 키
중복 불가능
데이터의 유일성 보장
UNIQUE KEY 와의 다른점은 NULL 값으로 구분
NULL 값 불가능 only NOT NULL
~~~
create_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    ***CONSTRAINT student_id_pk primary key(id)
~~~

## UNIQUE KEY(고유키)
테이블 내 항상 유일해야 하는 값
중복 불가능
NULL 값 가능
PRIMARY KEY 와의 다른점은 NULL 값으로 구분
~~~
2. CREATE TABLE 테이블이름

(
    필드이름 필드타입,
    ...,
    [CONSTRAINT 제약조건이름] UNIQUE (필드이름)
)

CREATE TABLE Test 
(
    ID INT UNIQUE,
    Name VARCHAR(30),
    ReserveDate DATE,
    RoomNum INT
);
~~~
## Foreign Key(왜래키)
참조키(외래키)는 컬럼이름이 중요한게아니라, 데이터 타입이 같아야한다. 
### 조건:참조할려는 컴럼은 고유한 데이터를 가진 컬럼이여야 한다.
~~~
ex) emp테이블에 deptno 컬럼이 있음.
    deptno컬럼은 dept테이블에서!
    emp테이블(자식)은 dept(부모)을 참조하고 있음.
CREATE TABLE 테이블_이름 
(
    empno int(11),
    ename VARCHAR(20),
    detpno int(5)
    foreign key(deptno) references dept(deptno)
)
~~~
## 조건
### on delete : 부모 데이터에 삭제 이벤트가 발생하면 자식 데이터에 이벤트 발생
### on update  : 부모 데이터에 수정 이벤트가 발생하면 자식 데이터에 이벤트 발생
~~~
ex)foreign key(deptno) references dept(deptno)  on delete
~~~
## 이벤트종류 : 아래중 하나 선택
### 1. CASCADE : 자식 데이터 삭제or수정
### 2. set null : 자식 데이터 null 업데이트
### 3. set DEFAULT : 자식 데이터 default 값으로 없데이트
### *4. restrict(default) : 부모 데이터 삭제 or 수정 불가능
### 5. NO ACTTON : 자식 테이블의 데이터는 변경되지 않습니다.
~~~
ex) ex)foreign key(deptno) references dept(deptno)  on delete CASCADE
~~~

## EXISTS
### 1.EXISTS () : TRUE면 실행
### 2. NOT EXISTS () : FALS 면 실행
EXISTS 안에 JOIN을 사용하면 EXISTS에서 조인한 내용이 나온다.

# 그룹함수
테이블의 전체 행을 하나 이상의 컬럼을 기준으로 컬럼값에 따라 그룹화하여 그룹별로 결과를 출력하는 함수이고 복수행 함수라고도 한다.
## ROLLUP 함수
- 추가적인 집계 정보를 보여주는 함수이다.

- ROLLUP 절에 명시할 수 있는 표현식에는 그룹핑 대상, SELECT 리스트에서 집계함수를 제외한 컬럼의 표현식이 올 수 있다. 명시한 표현식 수와 순서에 따라 레벨 별로 집계한 결과가 변환된다.

- ROLLUP 함수를 쓰지 않고 데이터를 나열할 경우 일일이 전부 조건들에 맞춰 나열해야 한다.
~~~
SELECT 
    상품ID, 
    월, 
    \SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY ROLLUP(상품ID, 월);
~~~

##  CUBE 함수
- ROLLUP과 비슷하지만 개념이 다르다.

- ROLLUP은 레벨 별로 순차적 집계를 했다면 CUBE는 명시한 표현식 개수에 따라 가능한 모든 조합별로 집계한 결과를 반환한다.
~~~
SELECT 
    출력할 칼럼, 
    COUNT(*) AS COUNT 
FROM 테이블명 
GROUP BY CUBE(그룹으로 묶을 것);
~~~

## GROUPING SETS 함수
다양한 소계 집합을 만드는 함수 
GROUP 문장을 여러 반복하지 않아도 원하는 결과를 쉽게 얻을수 있다.
~~~
SELECT 
    job , 
    deptno , 
    COUNT(*) cnt 
FROM emp 
GROUP BY GROUPING SETS((job,deptno)
~~~
-  job 컬럼별 소계와 deptno 컬럼별 소계의 쿼리를 합친 것과 같은 결과가 조회된다.

-  SELECT ... GROUP BY job UNION ALL SELECT ... GROUP BY deptno 와 동일
~~~
SELECT 
    job , 
    deptno , 
    COUNT(*) cnt 
FROM emp 
GROUP BY GROUPING SETS((job,deptno),())
~~~
-  빈괄호( )를 추가하여 합계를 표시할 수 있다.

-  빈괄호(빈 괄호( )가 아닌 NULL, ' ' 등을 넣어도 합계가 나오지만 빈 괄호( )를 권장한다.

-  GROUPING 함수는 해당 컬럼의 값이 NULL이면 1, 값이 있으면 0을 리턴 한다.

-  GROUPING SETS를 사용하여 생긴 NULL 컬럼만 구별하며, 원 컬럼의 데이터 값이 NULL이면 0을 리턴한다.

# 윈도우 함수
## WINDOW FUNCTION 개요
+ 행과 행 간의 관계를 쉽게 정의하기 위해 만든 함수가 윈도우 함수다. 

+ 윈도우 함수는 분석 함수나 순위 함수로도 알려져 있다. 

+ 윈도우 함수는 기존에 사용하던 집계 함수도 있고, 새로이 윈도우 함수 전용으로 만들어진 기능도 있다. 

+ 윈도우 함수는 다른 함수와 달리 중첩해서 사용은 못하지만, 서브쿼리에는 사용할 수 있다. 

## WINDOW FUNCTION 종류
+ 그룹 내 순위(RANK) 관련 함수: RANK, DENSE_RANK, ROW_NUMBER

+ 그룹 내 집계(AGGREGATE) 관련 함수 : SUM, MAX, MIN, AVG, COUNT (sql server는 OVER 절의 OREDER BY 지원 X)

+ 그룹 내 행 순서 관련 함수 : FIRST_VALUE, LAST_VALUE, LAG, LEAD (오라클에서만 지원)

+ 그룹 내 비율 관련 함수 : CUME_DIST, PERCENT_RANK, NTILE, RATIO_TO_REPORT 
~~~
SELECT WINDOW_FUNCTION (ARGUMENTS) OVER 
( [PARTITION BY 컬럼] [ORDER BY 컬럼] [WINDOWING 절] )
FROM 테이블명 ; 
~~~
+ WINDOW_FUNCTION : 윈도우 함수 

+ ARGUMENTS(인수) : 함수에 따라 0 ~ N개 인수가 지정될 수 있다. 

+ PARTITION BY 절 : 전체 집합을 기준에 의해 소그룹으로 나눌 수 있다.  

+ WINDOWING 절 : WINDOWING 절은 함수의 대상이 되는 행 기준의 범위를 강력하게 지정할 수 있다. (sql server 에서는 지원하지 않음)

## RANK

순위를 구하는 함수.

특정 범위(PARTITION) 내에서 순위를 구할 수도 있고, 전체 데이터에 대한 순위를 구할 수도 있다. 

동일한 값에 대해서는 동일한 순위를 부여하게 된다. 
~~~
SELECT   JOB, ENAME, SAL,
         RANK() OVER (ORDER BY SAL DESC) ALL_RANK,  -- 급여 높은 순
         RANK() OVER (PARTITION BY JOB ORDER BY SAL DESC) JOB_RANK -- job 별로 급여 높은 순
FROM     EMP ;        
~~~
 동일한 급여가 있다면 같은 순위를 부여한다. 

PARTITION 으로 구분한 JOB_RANK 는 같은 업무 범위 내에서만 순위를 부여한다. 

## DENSE_RANK

RANK와 흡사하지만, 동일한 순위를 하나의 건수로 취급한다. 

RANK는 1, 2, 3순위로 표기하지만, DENSE_RANK는 1, 1, 3 순위를 부여한다. (1위가 두개 이니 2위 없이 3위로 넘어감)

~~~
SELECT   JOB, ENAME, SAL,
         RANK()       OVER (ORDER BY SAL DESC) ALL_RANK,  
         DENSE_RANK() OVER (ORDER BY SAL DESC) DENSE_RANK 
FROM     EMP ; 
~~~

## ROW_NUMBER

RANK, DENSE_RANK 가 동일한 값에 대해서는 동일한 순위를 부여하는데 반해, 

ROW_NUMBER 는 동일한 값이라도 고유한 순위를 부여한다. 

~~~
SELECT   JOB, ENAME, SAL,
         RANK()       OVER (ORDER BY SAL DESC) ALL_RANK,  
         ROW_NUMBER() OVER (ORDER BY SAL DESC) ROW_NUMBER
FROM     EMP ;   
~~~
ROW_NUMBER 는 동일한 순위를 배제하기 위해 유니크한 순위를 정한다. 

## 그룹 내 행 순서 관련 함수
### FIRST_VALUE
파티션별 윈도우에서 가장 먼저 나온 값을 구할 수 있다.

MIN 함수를 이용해도 같은 결과를 얻을 수 있음.
~~~
SELECT  DEPTNO, ENAME, SAL,
        FIRST_VALUE(ENAME) OVER (PARTITION BY DEPTNO ORDER BY SAL DESC
        ROWS UNBOUNDED PRECEDING) DEPT_RICH
FROM    EMP 
~~~
같은 급여를 받는 사람이 있다면 정렬을 지정해야 한다. 

FIRST_VALUE 는 공동 등수를 인정하지 않고 처음 나온 행을 처리하기 때문이다. 

### LAST_VALUE
파티션별 윈도우에서 가장 먼저 나중에 나온 값을 구할 수 있다.

MAX 함수를 이용해도 같은 결과를 얻을 수 있음.
~~~
SELECT  DEPTNO, ENAME, SAL,
        LAST_VALUE(ENAME) OVER (PARTITION BY DEPTNO ORDER BY SAL DESC
        ROW BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) DEPT_POOR
FROM    EMP ; 

ROW BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
-- 현재 행을 포함해서 파티션 내의 마지막 행까지의 범위를 지정한다. 
~~~

### LAG
파티션별 윈도우에서 이전 몇 번째 행의 값을 가져올 수 있다. 

~~~
SELECT  ENAME, HIREDATE, SAL,
        LAG(SAL) OVER (ORDER BY HIREDATE) PREV_SAL
FROM    EMP
WHERE   JOB = 'SALESMAN' ;
~~~

~~~
[실행 결과] 
ENAME   HIREDATE  SAL  PREV_SAL 
------- --------- ---- ------- 
ALLEN   1981-02-20 1600 
WARD    1981-02-22 1250 1600 
TURNER  1981-09-08 1500 1250 
MARTIN  1981-09-28 1250 1500 

4개의 행이 선택되었다.
~~~

LAG 함수는 3개의 인수까지 사용할 수 있다. 

LAG(SAL, 2, 0) <-- 두번째 인수는 몇 번째 앞의 행을 가져올지 결정하는 것이고 (디폴트는 1. 여기서는 2을 지정했으니까 2번째 앞에 있는 행을 가져오는 것), 세번째 인수는 파티셧 첫 번째 행의 경우 가져올 데이터가 없어 NULL 값이 들어오는데, 이 경우 다른 값으로 바꾸어줄 수 있다. NVL/ ISNULL 과 같은 기능.
~~~
SELECT  ENAME, HIREDATE, SAL,
        LAG(SAL,2,0) OVER (ORDER BY HIREDATE) PREV_SAL
FROM    EMP
WHERE   JOB = 'SALESMAN' ;

-- LAG(SAL,2,0) : 두 행 앞의 급여를 가져오고, 가져올 값이 없으면 0으로 처리하라.
~~~

~~~
[실행 결과] 
ENAME   HIREDATE  SAL  PREV_SAL 
------- --------- ---- ------- 
ALLEN   1981-02-20 1600 0
WARD    1981-02-22 1250 0 
TURNER  1981-09-08 1500 1600 
MARTIN  1981-09-28 1250 1250 

4개의 행이 선택되었다.
~~~

### LEAD 
파티션별 윈도우에서 이후 몇 번째 행의 값을 가져올 수 있다. 

~~~
SELECT  ENAME, HIREDATE,
        LEAD(HIREDATE) OVER (ORDER BY HIREDATE) NEXTHIRED
FROM    EMP ;
~~~

~~~
[실행 결과] 
ENAME    HIREDATE   NEXTHIRED 
-------- ---------  --------- 
ALLEN    1981-02-20 1981-02-22 
WARD     1981-02-22 1981-04-02 
TURNER   1981-09-08 1981-09-28 
MARTIN   1981-09-28 

4개의 행이 선택되었다.
~~~
LAG 처럼 LEAD 함수도 3개의 인수까지 사용할 수 있다. 

## 그룹 내 비율 관련 함수

### CUME_DIST
파티션별 윈도우의 전체건수에서 현재 행보다 작거나 같은 건수에 대한 누적백분율을 구한다. 

~~~
SELECT  DEPTNO, ENAME, SAL,
        CUME_DIST() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) CUME_DIST
FROM    EMP ;
~~~

### PERCENT_RANK
파티션별 함수를 이용해서 파티션별 윈도우에서 제일 먼저 나오는 것을 0으로, 제일 늦게 나오는 것을 1로 하여, 행의 순서별 백분율을 구한다.

~~~
SELECT  ENAME, SAL,
        PERCENT_RANK() OVER (PARTITION BY DEPTNO ORDER BY SAL DESC) P_R
FROM    EMP ;
~~~

~~~
[실행 결과] 
DEPTNO  ENAME SAL  P_R 
------ ------ ---- ---- 
10      KING   5000 0 
10      CLARK  2450 0.5 
10      MILLER 1300 1 
20      SCOTT  3000 0 
20      FORD   3000 0 
20      JONES  2975 0.5 
20      ADAMS  1100 0.75 
20      SMITH  800  1 
30      BLAKE  2850 0 
30      ALLEN  1600 0.2 
30      TURNER 1500 0.4 
30      MARTIN 1250 0.6 
30      WARD   1250 0.6 
30      JAMES  950  1 
~~~
DEPTNO 10의 경우 3건 이므로 구간을 2가 된다. 

0과 1 사이를 2개의 구간으로 나누면 0, 0.5, 1이 된다. 

DEPTNO 20의 경우 5건 이고, 구간은 4.

0과 1 사이를 4개 구간으로 나누면 0, 0.25, 0.5, 0.75, 1이 된다. 

DEPTNO 30의 경우 6건 이고, 구간은 5.

0과 1 사이를 5개 구간으로 나누면 0, 0.2, 0.4, 0.6, 0.8, 1이 된다. 

### NTILE
파티션별 전체 건수를 ARGUMENT 값으로 N등분한 결과를 구할 수 있다. 
~~~
SELECT  ENAME, SAL,
        NTILE(4) OVER (ORDER BY SAL DESC) QUAR_TILE
FROM    EMP ;
~~~

전체 사원을 급여가 높은 순서로 정렬하고, 급여를 기준으로 4개 그룹으로 분류한다. 

~~~
[실행 결과] 
DEPTNO ENAME    SAL  QUAR_TILE 
------ ------- ---- -------- 
10     KING    5000 1 
10     FORD    3000 1 
10     SCOT    3000 1 
20     JONES   2975 1 
20     BLAKE   2850 2 
20     CLARK   2450 2 
20     ALLEN   1600 2 
20     TURNER  1500 2 
30     MILLER  1300 3 
30     WARD    1250 3 
30     MARTIN  1250 3 
30     ADAMS   1100 4 
30     JAMES   950  4 
30     SMITH   800  4 
~~~
 NTILE(4) 의 의미는 14명의 팀원을 4개 조로 나눈다는 의미이다. 

14명을 4개의 집합으로 나누면 몫이 3, 나머지가 2가 된다. 

나머지 두 명의 앞의 조부터 할당된다. 

### RATIO_TO_REPORT 
파티션 내 전체 SUM(컬럼) 값에 대한 행별 컬럼 값의 백분율을 소수점으로 구할 수 있다. 
결과값은 > 0 & <= 1 의 범위를 가진다. 
~~~
SELECT  ENAME, SAL,
        ROUND (RATIO_TO_REPORT(SAL) OVER (), 2) P_R
FROM    EMP
WHERE   JOB = 'SALESMAN' ;
~~~
~~~
[실행 결과] 
ENAME  SAL   R_R 
------ ---- ---- 
ALLEN  1600 0.29 (1600 / 5600) 
WARD   1250 0.22 (1250 / 5600) 
MARTIN 1250 0.22 (1250 / 5600) 
TURNER 1500 0.27 (1500 / 5600) 
~~~


