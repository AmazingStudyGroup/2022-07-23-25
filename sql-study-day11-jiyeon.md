# 08. 여러 테이블을 하나의 테이블처럼 사용하는 조인
### 08-1 조인
#### 집합 연산자와 조인의 차이점
조인은 두 개 이상의 테이블을 연결하여 하나의 테이블처럼 출력할 때 사용하는 방식.     
집합연산자와 비슷하게 느껴지지만, 집합 연산자를 사용한 결과는 두 개 이상 SELECT문의 결과 값을 **세로로 연결** 한 것이고,
조인을 사용한 결과는 두 개 이상의 테이블 데이터를 **가로로 연결**한 것이라고 볼 수 있다. 

#### 여러 테이블을 사용할 때의 FROM절
- 테이블 형태, 즉 열과 행으로 구성된 데이터 집합이면 모두 FROM절에 지정 가능 EX_ 뷰, 서브쿼리
- SELECT절의 여러 열을 구분할 때와 마찬가지로 FROM 절에 여러 테이블을 명시할 때 쉼표(,)를 구분자로 사용하여 지정한다.

```SQL
SELECT 
  FROM 테이블1, 테이블2, 테이블3 ...
```
#### 조인 조건이 없을 때의 문제점
정확하지 않은 결과를 얻게 될 수 있다.
```SQL
-- 특정 열 값이 같은 데이터를 출력하는 방법이 대표적인 조인 방식이다.
-- 열 이름을 비교하는 조건식으로 조인하기
SELECT *
  FROM EMP, DEPT
 WHERE EMP.DEPTNO = DEPT.DEPTNO
ORDER BY EMPNO;
```

#### 테이블의 별칭 설정
```SQL
FROM 테이블 이름1 별칭1, 테이블 이름2, 별칭2 ..
```

### 08-2 조인종류
#### 등가 조인(equi join)
= 내부 조인, 단순조인 (일반적으로 가장 많이 사용되는 조인 방식)    
테이블을 연결한 후에 출력 행을 각 테이블의 특정 열에 일치한 데이터를 기준으로 선정하는 방식    

- 여러 테이블의 열 이름이 같을 때 유의점     
등가조인 사용시, 조인 조건이 되는 각 테이블의 열 이름이 같을 경우에 해당 열 이름을 테이블 구분없이
명시하면 오류가 발생한다.
```SQL
-- 오류가 발생하는 예
SELECT EMPNO, ENAME, DEPTNO, DNAME, LOC
  FROM EMP E, DEPT D
 WHERE E.DEPTNO = D.DEPTNO;
```

- WHERE절에 조건식 추가하여 출력 범위 설정하기     
```SQL
SELECT EM.EMPNO, E.ENAME, E.SAL, D.DEPTNO, D.DNAME, D.LOC
  FROM EMP E, DEPT D
 WHERE E.DEPTNO = D.DEPTNO
  AND SAL >- 3000;
```

- 조인 테이블 개수와 조건식 개수의 관계      
데카르트 곱 현상이 일어나지 않게 하는 데 필요한 조건식의 최소 개수는 
조인 테이블 개수에서 하나를 뺀 값이다.     
```
A와 B 테이블을 조인 -> A와 B를 정확히 연결해주는 열이 하나 필요       
A, B, C 테이블 조인 -> A와 B가 연결된 상태에서 C를 연결해 줄 열 하나가 추가로 더 필요
```

#### 등가 조인(equi join)
등가 조인 **외의** 방식    
SALGRADE 테이블 : 각 급여 등급의 기준이 되는 최소금액 및 최대금액을 저장       

```SQL
-- 급여 범위를 지정하는 조건식으로 조인하기
SELECT *
  FROM EMP E, SALGRADE S
 WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL;
```

#### 자체 조인
하나의 테이블을 여러 개의 테이블처럼 활용하여 조인하는 방식.     
물리적으로 동인한 테이블 여러 개를 사용할 떄 발생할 수 있는 문제점을 해결한다.      
```SQL
-- 같은 테이블을 두 번 사용하여 자체 조인하기(테이블의 별칭만 다르게 지정)
SELECT E1.EMPNO, E1.ENAME, E1.MGR
                 E2.EMPNO AS MGR_EMPNO,
                 E2.ENAME AS MGR_ENAME
            FROM EMP E1, EMP E2
           WHERE E1.MGR = E2.EMPNO;
```

#### 외부 조인
두 테이블 간 조인 수행에서 조인 기준 열의 어느 한쪽이 NULL이어도 강제로 출력하는 방식      
외부 조인은 좌우를 따로 나누어 지정하는데, WHERE절에 조인 기준 열 중 한쪽에 (+) 기호를 붙여준다.     
- 왼쪽 외부 조인 : ```WHERE TABLE1.COL1 = TABLE2.COL1(+)```      
- 오른쪽 외부 조인 : ```WHERE TABLE1.COL1(+) = TABLE2.COL1```     
>(+) 기호를 붙이는 외부 조인 방식으로는 양쪽 모든 열이 외부 조인되는 '전체 외부 조인' 사용은 불가능하다.
>8-2절에서 살펴볼 예정