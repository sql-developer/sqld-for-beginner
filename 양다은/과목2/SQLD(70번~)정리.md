### 220224

- ### ROLLUP
```
ROLLUP

```
- ### EXISTS
``` 
EXISTS는 join의 여부에 따라 해석이 조금 다르다.

1. join 없을 때
메인쿼리에 emp테이블이 있기때문에 서브쿼리의 JOIN조건은 메인쿼리에도 영향을 줌(***서브쿼리 WHERE절에 참조키가 중요하다!!!)
EXISTS : 서브쿼리문이 true면 메인쿼리 출력 
NOT EXISTS : 서브쿼리문 false면 메인쿼리 출력

SELECT e.ename
FROM emp as e
WHERE exists(
SELECT *
FROM dept as d
)
    


    JOIN이 있을 경우엔 EXISTS의 조건들도 고려를 해서 데이터를 출력한다. 
    EXISTS : 서브쿼리에 있는 데이터에(조건)  해당하는 메인쿼리를 실행 
    NOT EXISTS : 서브쿼리에 있는 데이터를(조건)  제외하고 메인쿼리를 실행
```




- ### 참조를 하지 않아도 (join 조건이 없어도) join이 가능하다. 
만약, sangwon 컬럼을 insert했고 참조를 안했다고 가정

sangwon 테이블
select 
*
from emp e
inner join dept d
on e.sangwon = d.deptno

결과 : sangwon 컬럼이 추가되어 데이터가 조회된다.

실무 프로젝트시 참조키에 대한 제약조건이 많아서 작업이 어려울 경우
서로 참조하지 않고 업무를 진행하는 경우도 더러 있다.(참고하기)

