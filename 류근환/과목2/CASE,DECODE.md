# CASE
```sql

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
