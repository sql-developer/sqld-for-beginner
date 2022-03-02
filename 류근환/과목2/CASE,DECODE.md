# CASE(IF , IF ELSE , ELSE)

```sql
1. CASE '조건 컬럼명' WHEN '조건' THEN '결과값' ELSE '결과값' END
ex)
CASE ename WHEN 'SIMITH' THEN '스미스다'
           WHEN '홍길동' THEN '홍길동이다'
           WHEN '류근환' THEN '류근환이다'
           ELSE '내가 아니다'
           END as name

2. CASE WHEN '조건식' THEN '결과값' ELSE '결과값' END
ex)
CASE  WHEN ename='SIMITH' THEN '스미스다'
      WHEN ename='홍길동' THEN '홍길동이다'
      WHEN ename='류근환' THEN '류근환이다'
      ELSE '내가 아니다'
      END as name

```

# DECODE (if,else와 비슷한 기능을 한다.)

```sql
ex)
SELECT gender
     , DECODE(gender, 'M', '남자', 'F', '여자', '기타') gender2
  FROM temp

 =

풀이) gender라는 컬럼의 데이터가 'M'이라면 '남자' 'F'라면 '여자' 아니라면 '기타'를 리턴한다.
        if(gender == 'M')       if(gender == 'F')       else 기타
        return '남자'             return '여자'
```
