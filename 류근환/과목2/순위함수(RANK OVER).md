# 오라클 순위함수

```sql
RANK(),ROW_NUMBER(),DENSE_RANK() + OVER는 순위를 메기는 함수

1. RANK() OVER(ORDER BY '컬럼명')는 동일 순위인 경우 1,1,3,3,5 형식으로 출력
2. ROW_NUMBER() OVER(ORDER BY '컬럼명')는 동일 순위인 경우 1,2,3,4,5 형식으로 출력
3. DENSE_RANK() OVER(ORDER BY '컬럼명')는 동일 순위인 경우 1,1,2,2,3 형식으로 출력

**RANK() OVER() 와 DENSE_RANK() OVER()의 차이점
1. RANK() OVER()는 앞에 순위로 따지는 것이 아니라 데이터의 순서에 맞게 순위가 메겨짐
2. DENSE_RANK() OVER()는 앞에 순위에 따라 그 다음 순위로 메겨짐
```

## PARTITION by 함수

```sql
-- 1. 순위함수(OVER())와 함께 사용되는 그룹함수!
-- 2. GROUP BY의 대체제
-- **3. 분석함수를 사용할 때는 OVER 절을 함께 사용해야 하며, OVER 절 내부에 PARTITION BY 절을 사용하지 않으면 쿼리 결과 전체를 집계하며 PARTITION BY 절을 사용하면 쿼리 결과에서 해당 칼럼을 그룹으로 묶어서 결과를 표시한다.()
-- 4. 테이블의 로우(행, 레코드)를 그룹핑 하여 집계를 하는 함수
-- 5. 실행순서는 FROM->WHERE->GROUP BY->HAVING->SELECT->ORDER BY->OVER(마지막순서)
ex)
SELECT
    DEPTNO
    ,SAL
    ,SUM(SAL) OVER(PARTITION BY DEPTNO ORDER BY SAL DESC) AS SUM_SAL
FROM EMP;

(SELECT 3 = 합계급여를 DEPTNO로 그룹핑하여 급여의 오름차순으로 조회)
```
