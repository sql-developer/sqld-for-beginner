# DML (데이터 관련)

```sql
- SELECT
   데이터베이스에 들어 있는 데이터를 조회하거나 검색하기 위한 명령어를 말하는 것
- INSERT , UPDATE , DELETE
  1. 데이터베이스의 테이블에 들어 있는 데이터에 변형을 가하는 종류의 명령어들
  2. ex)테이블에 새로운 행을 집어 넣거나,원하지 않는 데이터를 삭제 or 수정
```

## DML-DELETE

```sql
1. DELETE FROM testTable WHERE idx=1;
(DELETE FROM '테이블명' WHERE '조건')
-- 제약 조건을 걸었을 경우

2. DELETE FROM testTable;
(DELETE FROM '테이블명')
-- testTable의 전체 값을 삭제
```

# DDL (테이블 관련)

- CREATE , ALTER , DROP , RENAME , TRUNCATE
  1. 테이블과 같은 데이터 구조를 정의하는데 사용되는 명령어
  2. 테이블의 구조를 생성(CREATE) or 변경(ALTER) or 삭제(DROP) or 이름변경(RENAME)

## DDL-CREATE 문법

```sql
 CREATE TABLE student(테이블 이름)(
            컬럼 이름        데이터타입(자릿수 지정 가능)           조건
               id                     INT(11)                 *NOT NULL *auto_increment,
              name                VARCHAR(20)                 NOT NULL,
              height                  INT(5),
              age                     INT(5)                  DEFAULT 0,
              create_at             DATETIME                  DEFAULT *CURRENT_TIMESTAMP,
              *CONSTRAINT student_id_pk primary key(id)
    );
  *auto_increment=생성 될때 마다 1씩 증감
  *NOT NULL=무조건 데이터를 넣어주어야함
  *CURRENT_TIMESTAMP=NOW()로 생성시간 자동 기록
  *CONSTRAINT=제약 조건
  *CONSTRAINT 컬럼명 primary key(PK가 될 컬럼명)
```

## DDL-ALTER

```sql
- ALTER
1. 컬럼 추가
ALTER TABLE `테이블명` ADD `컬럼명` 타입(길이) 제약조건
2. 컬럼 여러개 추가
ALTER TABLE '테이블명'
ADD COLUMN '컬럼명' 타입(길이) 제약조건,
ADD COLUMN '컬럼명' 타입(길이) 제약조건,
ADD COLUMN '컬럼명' 타입(길이) 제약조건,
.
.
.
3. 컬럼 맨앞에 추가
 ALTER TABLE `테이블명` ADD `새컬럼명` 타입(길이) 제약조건 FIRST

4. 컬럼 중간에 추가
  ALTER TABLE `테이블명` ADD `새컬럼명` 타입(길이) 제약조건 AFTER '앞컬럼명'

5. COLUMN 속성 변경
  ALTER TABLE '테이블명' ALTER COLUMN '컬럼명' 타입(길이) 제약조건

6. 테이블 내 불필요한 column(열) 삭제
  ALTER TABLE '테이블명' DROP '컬럼명'
```

## DDL-TRUNCATE

```sql
문법 - TRUNCATE TABLE '테이블명'
1. 테이블 자체가 삭제되는 것이 아니고, 해당 테이블에 들어있던 모든 행들이 제거되고 저장 공간을 재사용 가능하도록 해제한다.
2. 테이블 구조를 완전히 삭제하기 위해서는 DROP TABLE을 실행하면 된다.
3. 테이블을 최초 생성된 초기상태로 만듬.
```

# DCL(권한 관련)

```
- GRANT , REVOKE
  1. 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어
```

# TCL(트랜잭션)COMMIT,ROLLBACK

```
- 트랜잭션 특징
    1. 원자성(Atomicity)
      : 트랜잭션이 DB에 모두 반영되던가 아니면 전혀 반영이 안되던가

    2. 일관성(Consistency)
      : 트랜잭션의 작업 처리 결과가 항상 일관성 있어야 한다.

    3. 독립성(Isolation)
      : 둘 이상의 트랜잭션이 동시에 병행될 경우에 서로간의 트랜잭션 연산에 끼어들 수 없다.

    4. 지속성(Durability)
      : 트랜잭션이 성공적으로 완료됐다면 결과는 영구적으로 반영되어야 한다.
```

- ## COMMIT , ROLLBACK

```sql
  1. 논리적인 작업의 단위를 묶어서 DML에 의해 조작된 결과를 작업단위 별로 제어하는 명령어
  2. ROLLBACK -> (~~쿼리) commit; (~~쿼리)ROLLBACK / commit;과 ROLLBACK 사이의 코드들을 초기화(진행취소) 시킴
```

## ROLLBACK

```
- 작업 중 문제가 발생했을 때, 트랜잭션의 처리 과정에서 발생한 변경 사항을 취고하고, 트랜잭션 과정을 종료시킨다.
- 트랜잭션으로 인한 하나의 묶음 처리가 시작되기 이전의 상태로 되돌린다.
- TRANSACTION(INSERT, UPDATE, DELETE)작업 내용을 취소한다.
- 이전 COMMIT한 곳까지만 복구한다.
```

# KEY

1. PRIMARY KEY(기본키)

```
- 해당 테이블의 식별자 역할을 하는 제약조건으로 테이블에 하나만 설정할 수 있는 키
- \*테이블의 각 레코드를 구별할 수 있는 역할
- 프라이머리 키로 설정한 컬럼에서는 중복이 들어가면 안됨.(유일성 보장)
- \*NULL 값은 절대 허용 불가
```

2. UNIQUE KEY(고유키)

```
- 테이블 내 항상 유일해야 하는 값
- 중복 허용 안됨.
- 해당 칼럼에 입력되는 데이터가 각각 유일하다는 것을 보장하기 위한 제약조건
- \*NULL 값 허용
- PRIMARY KEY 와의 다른점은 NULL 값으로 구분
```

3.FOREIGN KEY(외래키)

```
- 테이블 내의 열 중 다른 테이블의 기본키(PRIMARY KEY)를 참조하는 열
```

## CASCADE

```
- foreign key 이벤트 종류 중 하나
- 자식테이블의 데이터 삭제 or 수정
- on delete or on update와 같이 사용
```

## char / varchar2

```
    char(30) : 고정길이 문자열 타입.
    => 2글자로 설정해도 크기는 그대로 30임, 타입의 크기만큼 데이터가 들어오지 않으면 남은 공간을 스페이스로 채워 넣음

    varchar2(30) : 가변길이 문자열 타입. (효율적으로 데이터 관리 가능)
    => 2글자 설정시 크기는 2임, 남은 공간을 스페이스로 채워 넣지 않음
```

## 내장 함수

```
- 다양한 사용자의 질의 요청을 처리하기 위해 MySQL이 SQL문 안에 제공하는 함수.
- SQL 함수는 DBMS(DataBase Managerment System))가 제공하는 내장 함수와 사용자가 직접 정의하는 사용자 정의 함수로 나뉘어집니다.
- ex)숫자함수,문자함수,날짜시간함수 등등
- https://mangkyu.tistory.com/25 (참고)
```

## 단일행 함수

```
- 하나의 행을 이용하여 결과를 출력하는 함수
- 데이터가 한 행씩 입력되고 입력된 한 행당 결과가 하나씩 나오는 함수
- SELECT, WHERE, ORDER BY, UPDATE SET 절에 사용 가능하다.
- 각 행(Row)들에 대해 개별적으로 작용하여 데이터 값들을 조작하고, 각각의 행에 대한 조작 결과를 리턴한다.
- 여러 인자를 입력해도 단 하나의 결과만 리턴한다.
- 함수의 인자로 상수, 변수, 표현식이 사용 가능하고, 하나의 인수를 가지는 경우도 있지만 여러개의 인수를 가질 수도 있다.
- 특별한 경우가 아니면 함수의 인자로 함수를 사용하는 함수의 중첩이 가능하다.
- 종류 : 문자형 함수, 날짜형 함수, 숫자형 함수, 변환형 함수, NULL관련 함수
```

## 다중행 함수

```
- 여러행을 이용하여 결과를 출력하는 함수
- 여러 행이 입력되어 하나의 행으로 결과가 나오는 함수
- 집계함수 : COUNT, AVG, SUM, MIN, MAX, STDDEV (표준편차), VARIANCE(분산)
- 그룹함수 : ROLLUP(소계), CUBE(모든 조합의 그룹별 소계), GROUPING SETS(다양한 소계 집합)
```
