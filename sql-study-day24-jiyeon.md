## 17 레코드와 컬렉션
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

### 17-1 자료형이 다른 여러 데이터를 저장하는 레코드



### 17-2 자료형이 같은 여러 데이터를 저장하는 레코드


