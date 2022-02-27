
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