## 44P 9번 

## FOREIGN KEY에 의한 Actions

#### 부모 테이블의 행이 삭제 될 때, 자식 테이블의 행에 어떤 일이 발생하는지 정의
- ON DELETE CASCADE = 부모를 삭제 하면, 자식도 삭제
- ON DELETE SET NULL = 부모를 삭제 하면, 자식은 NULL로 설정
- ON DELETE SET DEFAULT = 부모를 삭제 하면, 자식은 기본 값으로 설정
- ON DELETE RESTRICT = 자식이 없는 경우만 부모 삭제  

#### 자식 테이블의 행이 입력 될 때, 부모 테이블의 행과 관련해서 어떻게 할 것인지를 정의
- ON INSERT AUTOMATIC = 부모가 없을 때, 부모를 입력 후 자식 입력
- ON INSERT SET NULL = 부모가 없는 경우, 자식의 FK를 NULL로 설정
- ON INSERT SET DEFAULT = 부모가 없는 경우, 자식의 FK를 기본값으로 설정
- ON INSERT DEPENDENT = 부모의 PK가 있는 경우만 자식 입력  
  　  
---  
---  
  　  
## 52P 27번

## 트랜잭션의 특성
- Atomicity (원자성) = 트랜잭션 내의 모든 명령은 모두 완벽히 수행 돼야 하며, 어느 하나라도 에러가 발생하면 모두 취소 돼야 한다.
- Consistency (일관성) = 트랜잭션의 수행 전과 트랜잭션의 수행 완료 후의 데이터베이스 상태는 언제나 같아야 한다.
- Isolation (고립성 or 격리성) = 둘 이상의 트랜잭션이 동시에 실행되는 경우에 서로의 작업에 영향을 끼칠 수 없다.
- Durabilitly (지속성) = 성공적으로 수행이 완료 된 트랜잭션의 결과는 영구적으로 반영 돼야 한다.  
  　  
---  
---  
  　  
## 52P 28번

#### 트랜잭션에 대한 "격리성(Isolation)이 낮은 경우" = 트랜잭션간에 서로 영향을 많이 받는 경우

문제점
- Dirty Read = 다른 트랜잭션에 의해 수정 됐지만, 아직 commit 되지 않은 데이터를 읽는 것
- Non-Repeatable Read = 한 트랜잭션 내에서 같은 쿼리를 두 번 수행 했는데, 그 사이에 다른 트랜잭션이 값을 수정 또는 삭제하는 바람에 두 쿼리 결과가 다르게 나타나는 현상
- Phantom Read = 한 트랜잭션 내에서 같은 쿼리를 두 번 수행 했는데, 첫번째 쿼리에서 없던 유령 레코드가 두번째 쿼리에서 나타나는 현상  
　  
　  
---  
---  
  　  
## 59P 40번

## 단일행함수와 다중행함수

#### 단일 행 함수
- 추출 되는 각 행마다 작업을 수행
- 각 행마다 하나의 결과를 반환
- SELECT, WHERE, ORDER BY, UPDATE의 SET절에 사용 가능
- 데이터 타입 변경 가능
- 중첩해서 사용 가능

#### 다중 행 함수
- 여러개의 행이 입력, 하나의 값 반환
- 그룹(집계) 함수가 다중 행 함수
  ex) SUM, AVG, MAX, MIN, COUNT, ...  
  　  
---  
---  
  　  
## 65P 50번

#### 'AVG(COL3)'은, COL3의 값이 NULL인 것을 제외 함

- SELECT AVG(COL3) FROM TAB_A;  
  -> COL3의 값만 계산  
  -> (20+0)/2건=10
- SELECT AVG(COL3) FROM TAB_A WHERE COL1>0;  
  -> COL1이 0보다 큰 COL3 값을 계산  
  -> (20)/1건=20
- SELECT AVG(COL3) FROM TAB_A WHERE COL1 IS NOT NULL;  
  -> 'COL1에서 NULL이 아닌 것'과 'COL3에서 NULL이 아닌 것'을 계산  
  -> (20)/1건=20
