1. ROLLUP : 오른쪽 컬럼부터 왼쪽 컬럼까지 소계
2. CUBE : 모든 경우의 수 소계
3. GROUPING SETS : 지정한별로 소계

### <GROUPING SETS 예제>
```sql
SELECT A, B, SUM(VAL) FROM T
GROUP BY GROUPING SETS(A,B)

-- GROUP BY GROUPING SETS((A),(B)) 와 같음
-- A만 있는 소계, B만 있는 소계 출력
```
​
```sql
SELECT A, B, SUM(VAL) FROM T
GROUP BY GROUPING SETS((A),(A,B))

-- A만 있는 소계, A와 B가 있는 소계 출력
```
​
```sql
SELECT A, B, SUM(VAL) FROM T
GROUP BY GROUPING SETS((A),(B),(A,B))

-- A만 있는 소계, B만 있는 소계, A와 B가 있는 소계 출력
```
​
```sql
SELECT A, B, SUM(VAL) FROM T
GROUP BY GROUPING SETS((A),(B),(A,B),())

-- A만 있는 소계, B만 있는 소계, A와 B가 있는 소계, "()" 괄호 문자를 이용하면 합산 데이터를 받을 수 있음.
```