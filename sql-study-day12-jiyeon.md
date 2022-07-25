### 08-3 SQL-99 표준 문법으로 배우는 조인
다른 DBMS 제품에서도 사용할 수 있는 표준 문법     

#### NATURAL JOIN
```SQL
SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM,
       DEPTNO, D.DNAME, D.LOC
  FROM EMP E NATURAL JOIN DEPT D
ORDER BY DEPTNO, E.EMPNO;
```
> 기존 등가 조인과 다르게 조인 기준 열인 DEPTNO를 SELECT절에 명시할 때, **테이블 이름을 붙이면 안되는 특성**이 있으니 주의요망


#### JOIN ~ USING
기존 등가 조인을 대신하는 조인 방식.      
NATURAL JOIN이 자동으로 조인 기준 열을 지정하는 것과 달리, USING 키워드에 조인 기준으로 사용할 열을 명시하여 사용한다.
```FROM TABLE1 JOIN TABLE2 USING (조인에 사용할 기준열)```
> NATURAL JOIN과 마찬가지로 조인 기준 열로 명시된 열은 SELECT절에서 테이블 이름을 **붙이지 않고** 작성한다.    

```SQL
SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM,
       DEPTNO, D.DNAME, D.LOC
  FROM EMP E JOIN DEPT D USING (DEPTNO)
WHERE SAL >= 3000
ORDER BY DEPTNO, E.EMPNO;
```

#### JOIN ~ ON
가장 범용성 있는 조인 방식. 기존 WHERE절에 있는 조인 조건식을 ON키워드 옆에 작성한다.     
- 조인 기준 조건식은 ON에 명시하고, 그 밖의 출력행을 걸러 내기 위해 WHERE조건식을 따로 사용하는 방식    
```FROM TABLE1 JOIN TABLE2 ON (조인 조건식)```

```SQL
SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM,
       E.DEPTNO,
       D.DNAME, D.LOC
  FROM EMP E JOIN DEPT D ON (E.DEPTNO = D.DEPTNO)
WHERE SAL <= 3000
ORDER BY E.DEPTNO, EMPNO;
```

#### OUTER JOIN
외부 조인에 사용. 
- 왼쪽 외부 조인      
     - 기존 : WHERE TABLE1.COL1 = TABLE2.COL1(+)       
     - SQL-99 : FROM TABLE1 LEFT OUTER JOIN TABLE2 ON (조인 조건식)     
- 오른쪽 외부 조인      
     - 기존 : WHERE TABLE1.COL1(+) = TABLE2.COL1      
     - SQL-99 : FROM TABLE1 RIGHT OUTER JOIN TABLE2 ON (조인 조건식)      
- 전체 외부 조인      
     - 기존 : 기본 문법은 없음 (UNION 집합 연산자를 활용)      
     - SQL-99 : FROM TABLE1 FULL OUTER JOIN TABLE2 ON (조인 조건식)      

```SQL
-- 왼쪽 외부 조인을 SQL-99로 작성하기
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
  FROM EMP E1 LEFT OUTER JOIN EMP E2 ON (E1.MGR = E2.EMPNO)
ORDER BY E1.EMPNO;

-- 오른쪽 외부 조인을 SQL-99로 작성하기
SELECT E1.EMPNO, E1.ENAME, E1.MGR,
       E2.EMPNO AS MGR_EMPNO,
       E2.ENAME AS MGR_ENAME
  FROM EMP E1 RIGHT OUTER JOIN EMP E2 ON (E1.MGR = E2.EMPNO)
ORDER BY E1.EMPNO MGR_EMPNO
```







#### SQL-99 조인 방식에서 세 개 이상의 테이블을 조인할 때


