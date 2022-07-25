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
#### JOIN ~ ON
#### OUTER JOIN
#### SQL-99 조인 방식에서 세 개 이상의 테이블을 조인할 때


