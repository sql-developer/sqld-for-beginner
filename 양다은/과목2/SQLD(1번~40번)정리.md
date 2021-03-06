### 220217
    안녕하세요 쌤~!(인사 안되면 지우겠습니다,,,)
    답장 : Hello ~ 지우지 마요 ~~~ yo!

- DML (데이터 조작어)  

      SELECT
      INSERT
      UPDATE
      DELETE

    
- DDL (데이터 정의어)

      CREAT
      ALTER
      DROP


- DCL (데이터 제어어)
    
      GRANT
      REVOKE
      COMMIT
      ROLLBACK

- UINQUE(유일키) 

      테이블 내 중복값이 없으며 NULL 입력이 가능하다.

- INDEX 
      
      데이터의 크기가 클 때 (많을 때) 조회할 데이터를 따로 보관한다. 풀스캔을 할 필요가 없음
        
- ALTER 
      데이터의 삭제/수정/RENAME시 사용

        <테이블의 불필요한 컬럼삭제하기 - ORACLE>
        
        ex)
        ALTER TABLE
        테이블명
        DROP COLUMN 삭제할 컬럼명

- DEPNEDENT

      Child Table 데이터 입력을 허용하지 않는다.

### char / varchar2
```
    char(30) : 고정길이 문자열 타입.
    => 2글자로 설정해도 크기는 그대로 30임, 타입의 크기만큼 데이터가 들어오지 않으면 남은 공간을 스페이스로 채워 넣음

    varchar2(30) : 가변길이 문자열 타입. (효율적으로 데이터 관리 가능)
    => 2글자 설정시 크기는 2임, 남은 공간을 스페이스로 채워 넣지 않음
```   

### 트랜잭션(COMMIT, ROLLBACK)
    : 데이터베이스 내에서 하나의 그룹으로 처리되어야 하는 명령문들을 모아 놓은 논리적인 작업 단위

    쉽게 말해, 
    트랜잭션에는 쿼리문을 DB로 반영하는 'COMMIT'과 
    실패했을 때 COMMIT시점으로 되돌아가는 'ROLLBACK'이 있다.
    *ROLLBACK : DML 명령어 작업들을 마지막 commit전까지 취소시킴

    -트랜잭션을 쓰는 이유?
    : 데이터의 일관성을 유지, 안정적으로 데이터 복구가 가능

    ex) 타행으로 송금 도중 오류가 발생하면 아예 처음부터 없던 거래로 되돌리는 것. (처리 과정이 모두 성공했을 때만 최종 DB에 반영함)

### 트랜잭션의 특징
    1. 원자성(Atomicity) 
      : 트랜잭션이 DB에 모두 반영되던가 아니면 전혀 반영이 안되던가

    2. 일관성(Consistency)
      : 트랜잭션의 작업 처리 결과가 항상 일관성 있어야 한다.

    3. 독립성(Isolation)
      : 둘 이상의 트랜잭션이 동시에 병행될 경우에 서로간의 트랜잭션 연산에 끼어들 수 없다.

    4. 지속성(Durability)
      : 트랜잭션이 성공적으로 완료됐다면 결과는 영구적으로 반영되어야 한다.

### SAVEPOINT
    : 임시저장
    ROLLBACK 실행시 COMMIT전까지 모든 DML이 취소되는데 
    특정 부분에서 SAVEPOINT를 지정하고 
    ROLLBACK을 실행하면 그 지점까지만 취소가 된다.( 부분취소의 개념)

    <사용법> 
    SAVEPOINT 세이브포인트이름
    ROLLBACK TO SAVEPOINT 

```sql
    ex) emp 테이블의 sal 데이터를 3번 DELETE 한다고 가정
    DELETE FROM emp WHERE sal = 2000;
    COMMIT;
    SAVEPOINT S1; 
    DELETE FROM emp WHERE sal = 3000;
    SAVEPOINT S2;
    DELETE FROM emp WHERE sal = 4000;
    ROLLBACK TO S2

    맨 마지막에 ROLLBACK TO S2를 선언했으니 
    SAVEPOINT S2(sal = 3000)까지 데이터가 조회됨
```
    
---
질문
-
Q1. 데이터 타입이 varchar2인 PK컬럼에 숫자형 데이터가 올 수 있나요?  (SQL 2과목 21번 문제/ 보기 1번)  
    상원) INSERT는 가능하나, SUM, AVG 같은 집계 함수사용 제약. 

Q2. DELETE와 TRUNCATE의 차이점 (삭제 후 데이터 행의 잔여 여부)  
    상원) DELETE는 DML중 하나로 작업 후 항상 COMMIT을 해야 합니다. 하지만 TRUNCATE는 DDL언어로 간주하며, AUTO COMMIT 기능이 포함 되어있습니다.  
    상원) 둘 다 테이블에 있는 데이터가 삭제된다는 공통점이 있지만, DELETE를 사용하면 세밀한 삭제가 가능하다는 차이점이 있습니다.  

