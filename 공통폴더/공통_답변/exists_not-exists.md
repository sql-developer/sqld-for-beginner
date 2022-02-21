### EXISTS, NOT EXISTS
    둘다 WHERE 조건에 오며, 서브쿼리 결과에 따라 메인쿼리 실행 여부를 판단함.
    
### EXISTS
    EXISTS는 서브쿼리 결과가 있으면 TRUE 반환 후 메인쿼리 실행, FALSE면 실행하지 않는다.

```sql
-- 예제 코드 
-- 서브 쿼리는 sal(급여)가 3000 이상인 직원 번호 조회를 하고 있다.
-- 3000 이상 급여를 받는 직원이 있기 때문에 해당 서브쿼리는 TRUE을 반환 한다. 
SELECT
 ename,
 job
FROM emp
WHERE EXISTS (
	SELECT
	    empno
	FROM emp
	WHERE
        sal > 3000
)
```
```sql
-- 90000 이상 받는 직원이 없기 때문에 해당 쿼리는 실행되지 않는다.
SELECT
 ename,
 job
FROM emp
WHERE EXISTS (
	SELECT
	    empno
	FROM emp
	WHERE
        sal > 90000
)
```

### NOT EXISTS
    NOT EXISTS는 서브쿼리 결과가 없어서 FALSE를 반환하면 실행, TRUE면 실행하지 않는다.

```sql
-- 90000 이상 받는 직원이 없기 때문에 서브쿼리는 FALSE을 반환 한다. 
-- 서브쿼리 결과가 없어서 FALSE 반환. 때문에 메인쿼리가 실행 된다.
SELECT
 ename,
 job
FROM emp
WHERE NOT EXISTS (
	SELECT
	    empno
	FROM emp
	WHERE
        sal > 90000
)
```