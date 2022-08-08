## 17 레코드와 컬렉션

### 17-1 자료형이 다른 여러 데이터를 저장하는 레코드

레코드는 자료형이 각기 다른 데이터를 하나의 변수에 저장하는데 사용한다.    

```sql
TYPE 레코드 이름 IS RECORD(
 
변수이름 자료형 NOT NULL :=(또는 DEFAULT) 값 또는 값이 도출되는 여러 표현식 
 
)
```
> C, C++, Java 같은 프로그래밍 언어의 구조체, 클래스 개념과 비슷하다.  

```sql
DECLARE
  TYPE REC_DEPT IS RECORD(
    deptno NUMBER(2) NOT NULL := 99,
    dname DEPT.DNAME%TYPE,
    loc DEPT.LOC%TYPE 
  );
  dept_rec REXT_DEPT;
BEGIN
  dept_rec.deptno := 99;
  dept_rec.dname := 'DATABASE';
  dept_rec.loc := 'SEOUL';
  DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || dept_rec.deptno);
  DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || dept_rec.dname);
  DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || dept_rec.loc);
END;
/
```

#### 레코드를 사용한 INSERT
```SQL
-- DEPT_RECORD 테이블 생성하기
CREATE TABLE DEPT_RECORD
AS SELECT * FROM DEPT;

-- 레코드를 사용하여 INSERT하기
DECLARE
  TYPE REC_DEPT IS RECORD(
    deptno NUMBER(2) NOT NULL := 99,
    dname DEPT.DNAME%TYPE,
    loc DEPT.LOC%TYPE
  );
    dept_rec REC_DEPT;
 BEGIN
    dept_rec.deptno := 99;
    dept_rec.dname := 'DATABASE';
    dept_rec.loc := 'SEOUL';
    
INSERT INTO DEPT_RECORD
VALUES dept_rec;
END;
/
```

#### 레코드를 사용한 UPDATE
- 기존 UPDATE문에서는 SET절을 통해 변경할 열을 하나하나 지정한 것과 달리, 레코드에 저장된 데이터를 사용하여 행 전체의 데이터를 바꿔준다.     

```SQL
-- 레코드를 사용하여 UPDATE하기
DECLARE
  TYPE REC_DEPT IS RECORD(
    deptno NUMBER(2) NOT NULL := 99,
    dname DEPT.DNAME%TYPE,
    loc DEPT.LOC%TYPE
  );
    dept_rec REC_DEPT;
 BEGIN
    dept_rec.deptno := 50;
    dept_rec.dname := 'DB';
    dept_rec.loc := 'SEOUL';
    
    UPDATE DEPT_RECORD
    SET ROW = dept_rec
    WHERE DEPTNO = 99;
END;
/
```

### 17-2 자료형이 같은 여러 데이터를 저장하는 컬렉션
- 컬렉션은 특정 자료형의 데이터를 여러 개 저장하는 복합 자료형이다.     
- 연관배열, 중첩 테이블, VARRAY 등    

#### 연관 배열
- **키(KEY), 값(VALUE)** 으로 구성되는 컬렉션      
- 중복되지 않은 유일한 키를 통해 값을 저장하고 불러오는 방식을 사용한다.      

```SQL
TYPE 연관 배열 이름 IS TABLE OF 자료형 [NOT NULL]
INDEX BY 인덱스형;

-- 자료형에는 단일 자료형, 참조형 모두 가능
```

```SQL
DECLARE
  TYPE ITAB_EX IS TABLE OF VARCHAR2(20)
INDEX BY PLS_INTEGER;

  tex_arr ITAB_EX;
  
BEGIN
  tex_arr(1) := '1st data';
  tex_arr(2) := '2nd data';
  tex_arr(3) := '3rd data';
  tex_arr(4) := '4st data';
  
  DBMS_OUTPUT.PUT_LINE('text_arr(1) : ' || text_arr(1));
  DBMS_OUTPUT.PUT_LINE('text_arr(2) : ' || text_arr(2));
  DBMS_OUTPUT.PUT_LINE('text_arr(3) : ' || text_arr(3));
  DBMS_OUTPUT.PUT_LINE('text_arr(4) : ' || text_arr(4));
END;
/  
```

#### 컬렉션 메서드
- 컬렉션과 관련된 다양한 정보 조회 기능을 제공     
- 컬렉션 메서드는 컬렉션형으로 선언한 변수에 마침표와 함께 작성하여 사용할 수 있음     

```sql
DECLARE
   TYPE ITAB_EX IS TABLE OF VARCHAR2(20)
INDEX BY PLS_INTEGER;

   text_arr ITAB_EX;

BEGIN
  tex_arr(1) := '1st data';
  tex_arr(2) := '2nd data';
  tex_arr(3) := '3rd data';
  tex_arr(50) := '50st data';
  
  DBMS_OUTPUT.PUT_LINE('text_arr.COUNT : ' || text_arr.COUNT);
  -- 컬렉션에 포함되어 있는 요소 개수를 반환
  
  DBMS_OUTPUT.PUT_LINE('text_arr.FIRST : ' || text_arr.FIRST);
  -- 컬렉션의 첫번째 인덱스 번호를 반환
  
  DBMS_OUTPUT.PUT_LINE('text_arr.LAST : ' || text_arr.LAST);
  -- 컬렉션의 마지막 인덱스 번호를 반환
  
  DBMS_OUTPUT.PUT_LINE('text_arr.PRIOR(50) : ' || text_arr.PRIOR(50));
  -- 컬렉션에서 50 인덱스 바로 앞 인덱스 값을 반환 (없으면 NULL)
  
  DBMS_OUTPUT.PUT_LINE('text_arr.NEXT(50) : ' || text_arr.NEXT(50));
  -- 50번 인덱스 다음 이넥스가 존재하지 않으므로 NULL이 반환되어 비어있는 상태로 출력
END;
/    
```

#### 잊기 전에 한번더
```sql

-- Q1
-- 1)
CREATE TABLE EMP_RECORD
    AS SELECT * 
         FROM EMP
        WHERE 1<>1;

-- 2)
DECLARE
   TYPE REC_EMP IS RECORD (
      empno    EMP.EMPNO%TYPE NOT NULL := 9999,
      ename    EMP.ENAME%TYPE,
      job      EMP.JOB%TYPE,
      mgr      EMP.MGR%TYPE,
      hiredate EMP.HIREDATE%TYPE,
      sal      EMP.SAL%TYPE,
      comm     EMP.COMM%TYPE,
      deptno   EMP.DEPTNO%TYPE
   );
   emp_rec REC_EMP;
BEGIN
   emp_rec.empno    := 1111;
   emp_rec.ename    := 'TEST_USER';
   emp_rec.job      := 'TEST_JOB';
   emp_rec.mgr      := null;
   emp_rec.hiredate := TO_DATE('20180301','YYYYMMDD');
   emp_rec.sal      := 3000;
   emp_rec.comm     := null;
   emp_rec.deptno   := 40;

   INSERT INTO EMP_RECORD
   VALUES emp_rec;
END;
/

-- Q3
DECLARE
   TYPE ITAB_EMP IS TABLE OF EMP%ROWTYPE
      INDEX BY PLS_INTEGER;
   emp_arr ITAB_EMP;
   idx PLS_INTEGER := 0;
BEGIN
   FOR i IN (SELECT * FROM EMP) LOOP
      idx := idx + 1;
      emp_arr(idx).empno    := i.EMPNO;
      emp_arr(idx).ename    := i.ENAME;
      emp_arr(idx).job      := i.JOB;
      emp_arr(idx).mgr      := i.MGR;
      emp_arr(idx).hiredate := i.HIREDATE;
      emp_arr(idx).sal      := i.SAL;
      emp_arr(idx).comm     := i.COMM;
      emp_arr(idx).deptno   := i.DEPTNO;

      DBMS_OUTPUT.PUT_LINE(
         emp_arr(idx).empno     || ' : ' ||
         emp_arr(idx).ename     || ' : ' ||
         emp_arr(idx).job       || ' : ' ||
         emp_arr(idx).mgr       || ' : ' ||
         emp_arr(idx).hiredate  || ' : ' ||
         emp_arr(idx).sal       || ' : ' ||
         emp_arr(idx).comm      || ' : ' ||
         emp_arr(idx).deptno);

   END LOOP;
END;
/
```

