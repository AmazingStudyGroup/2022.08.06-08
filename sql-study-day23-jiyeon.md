### 16-3 조건제어문
특정 조건식을 통해 상황에 따라 실행할 내용을 달리하는 방식의 명령어를 조건문이라고 한다.       


#### IF 조건문 
- IF-THEN : 특정 조건을 만족하는 경우 작업 수행        
- IF-THEN-ELSE : 특정 조건을 만족하는 경우와 반대 경우에 각각 지정한 작업 수행      
- IF-THEN-ELSIF : 여러 조건에 따라 각각 지정한 작업 수행      

##### IF-THEN
- 주어진 조건식의 결과값이 true인 경우에는 작업을 수행     
- false 또는 NULL일 경우에는 작업을 수행하지 않고 다음 내용을 실행     

```sql
IF 조건식 THEN 
  수행할 명령어;
END IF;
```

##### IF-THEN-ELSE
- 지정한 조건식의 결과값이 true일 경우에 실행할 명령어와 조건식의 결과값이 false일 경우 실행할 명령어를 각각 지정할 수 있다.     

```sql
IF 조건식 THEN 
  수행할 명령어;
ELSE
  수행할 명령어;
END IF;

```

##### IF-THEN-ELSIF
여러 종류의 조건을 지정하여 각 조건을 만족하는 경우마다 다른 작업의 수행을 지정하는 것이 가능하다.     

```sql
IF 조건식 THEN 
  수행할 명령어;
ELSIF 조건식
  수행할 명령어;
ELSIF 조건식
  수행할 명령어;
...
ELSE
  수행할 명령어;
END IF;
```

#### CASE 조건문
- 단순 CASE : 비교 기준이 되는 조건의 값이 여러 가지일 때 해당 값만 명시하여 작업 수행          
- 검색 CASE : 특정한 비교 기준 없이 여러 조건식을 나열하여 조건식에 맞는 작업 수행     

##### 단순 CASE
비교 기준(여러가지 결과값이 나올 수 있는)이 되는 변수 또는 식을 명시하고, 결과값에 따라 수행할 작업을 지정     

```sql
CASE 비교 기준
  WHEN 값1 THEN
    수행할 명령어;
  WHEN THEN
    수행할 명령어;
...
ELSE
    수행할 명령어;
END CASE;
```

##### 검색 CASE
비교 기준을 명시하지 않고 각각의 WHEN절에서 조건식을 명시한 후 해당 조건을 만족할 때 수행할 작업을 정해준다.    

```sql
CASE
  WHEN 조건식1 THEN
    수행할 명령어;
  WHEN 조건식2 THEN
    수행할 명령어;
...
ELSE
    수행할 명령어;
END CASE;
```
> SQL문의 CASE문과 PL/SQL문의 CASE문      
- SQL문의 CASE문은 조건에 따라 특정 결과값을 반환하는 것에 그침     
- PL/SQL문의 CASE조건문은 조건에 따라 수행할 작업을 지정할 수 있음    
- SQL문의 CASE는 END로 종료, PL/SQL문의 CASE 조건문은 END CASE로 종료     

### 16-4 반복 제어문
- 기본 LOOP     
- WHILE LOOP     
- FOR LOOP     
- Cusor FOR LOOP     

> 반복 수행을 중단시키거나 특정 반복 주기를 건너뛰는 명령어도 함께 살펴보자     
- EXIT : 수행중인 반복 종료     
- EXIT-WHEN : 반복 종료를 위한 조건식을 지정하고 만족하면 반복 종료     
- CONTINUE : 수행 중인 반복의 현재 주기를 건너뜀     
- CONTINUE-WHEN : 특정 조건식을 지정하고 조건식을 만족하면 현재 반복 주기를 건너뜀    

#### 기본 LOOP
반복을 위한 별다른 조건 없이 반복할 작업 내용을 지정해준다.     
```sql
LOOP
  반복 수행 작업;
END LOOP;
```
> 이렇게만 하면, 무한루프를 돌게 된다. 이를 방지하기 위해서 EXIT와 같은 명령어를 사용한다.     

```sql
DECLARE
  V_NUM NUMBER := 0;
BEGIN
  LOOP
    DBMS_OUTPUT.PUT_LINE('현재 V_NUM : ' || V_NUM);
    V_NUM := V_NUM + 1;
    EXIT WHEN V_NUM > 4;
  END LOOP;
END;
/
```

#### WHILE LOOP
반복 수행 여부를 결정하는 조건식을 먼저 지정한 후 조건식의 결과값이 true일 때 조건을 반복하고, false가 되면 반복 종료한다     
> 조건식의 결과값에 따라 단 한 번도 반복 수행되지 않을 수도 있음     

```sql
WHILE 조건식 LOOP
  반복 수행 작업;
END LOOP;
```

```sql
DECLARE
  V_NUM NUMBER := 0;
BEGIN
  WHILE V_NUM < 4 LOOP
    DBMS_OUTPUT.PUT_LINE('현재 V_NUM : ' || V_NUM);
    V_NUM := V_NUM + 1;
  END LOOP;
END;
/
```

#### FOR LOOP
반복의 횟수를 지정할 수 있는 반복문   
- i 는 반복수행 중의 시작값과 종료값 사이의 현재 숫자가 저장되는 특수한 변수로, counter라고 한다.    

```sql
FOR i IN 시작값...종료값 LOOP
  반복 수행 작업;
END LOOP;
```

```sql
BEGIN
  FOR i IN 0..4 LOOP
    DBMS_OUTPUT.PUT_LINE('현재 i의 값 : ' || i);
  END LOOP;
END;
/
```

#### CONTINUE문, CONTINUE-WHEN문
```SQL
BEGIN
  FOR i IN 0..4 LOOP
    CONTINUE WHEN MOD(i , 2) = 1;
    DBMS_OUTPUT.PUT_LINE('현재 i의 값 : ' || i);
  END LOOP;
END;
/ 
```

#### 잊기 전에 한번더!
```sql
-- Q1
BEGIN
  FOR i IN 0..10
    CONTINUE WHEN MOD(i, 2) = 0;
    DBMS_OUTPUT.PUT_LINE('현재 i의 값 : ' || i);
    END LOOP;
END;
/

-- Q2
DECLARE
  V_DEPTNO DEPT.DEPTNO%TYPE := 10;
BEGIN
  CASE V_DEPTNO
    WHEN 10 THEN DBMS_OUTPUT.PUT_LINE('DNAME : ACCOUNTING');
    WHEN 20 THEN DBMS_OUTPUT.PUT_LINE('DNAME : RESEARCH');
    WHEN 30 THEN DBMS_OUTPUT.PUT_LINE('DNAME : SALES');
    WHEN 40 THEN DBMS_OUTPUT.PUT_LINE('DNAME : OPERATIONS');
    ELSE         DBMS_OUTPUT.PUT_LINE('DNAME : N/A');
  END CASE;
END;
/
```
