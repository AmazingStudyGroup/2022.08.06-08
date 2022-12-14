## 17 - 1 자료형이 다른 여러 데이터를 저장하는 레코드

<br/>

> 레코드란?

- 레코드(record)는 자료형이 각기 다른 데이터를 하나의 변수에 저장하는 데 사용한다.

```sql
// 기본 형식
TYPE 레코드 이름 IS RECORD(
변수 이름 자료형 NOT NULL := (또는 DEFAULT) 값 또는 값이 도출되는 여러 표현식
)
// 1. 레코드 이름
// 2. 변수 이름
// 3. 자료형
// 4. NOT NULL
// 5. := (또는 DEFAULT) 값 또는 값이 도출되는 여러 표현식
```

| 번호 | 설명 |
|:-----:|:------------:|
| 1. | 저장할 레코드 이름을 지정합니다. |
| 2. | 레코드 안에 포함할 변수를 지정합니다. 변수는 여러 개 지정할 수 있으며 각 변수는 쉼표(,)로 구분합니다. |
| 3. | 지정한 변수의 자료형을 지정합니다. 이 자료형 역시 %TYPE, %ROWTYPE 지정이 가능합니다. |
| 4. | 지정한 변수에 NOT NULL 제약 조건을 지정합니다(생략 가능).
| 5. | 기본값을 지정합니다(생략 가능). |

- 정의한 레코드는 지금까지 다룬 변수와 마찬가지로 기존 자료형처럼 사용할 수 있다. 레코드에 포함된 변수는 레코드 이름과 마침표(.)로 사용할 수 있다.

<br/>

> 레코드를 사용한 INSERT

- PL/SQL문에서는 테이블에 데이터를 삽입하거나 수정하는 INSERT, UPDATE문에도 레코드를 사용할 수 있다.

<br/>

> 레코드를 사용한 UPDATE

- 레코드는 UPDATE문에도 사용할 수 있다. 이 경우에 SET절은 ROW 키워드와 함께 레코드 이름을 명시한다. 기존 UPDATE문에서는 SET절을 통해 변경할 열을 하나하나 지정한 것과 달리 레코드에 저장된 데이터를 사용하여 행 전체의 데이터를 바꿔 준다.

<br/>

## 17 - 2 자료형이 같은 여러 데이터를 저장하는 컬렉션

<br/>

- 컬렉션은 특정 자료형의 데이터를 여러 개 저장하는 복합 자료형이다. 여러 종류의 데이터를 하나로 묶어 사용하는 레코드를 테이블의 한 행처럼 사용한다면, 컬렉션은 열 또는 테이블과 같은 형태로 사용할 수 있다.

- PL/SQL에서 사용할 수 있는 컬렉션은 다음과 같이 세 가지 종류가 있다.

  1. 연관 배열(associative array (or index by table)
  
  2. 중첩 테이블(nested table)
  
  3. VARRAY(variable-size array)
  
  <br/>
  
> 연관 배열

- 연관 배열은 인덱스라고도 불리는 키(key), 값(value)으로 구성되는 컬렉션이다.

- 중복되지 않은 유일한 키를 통해 값을 저장하고 불러오는 방식을 사용한다.

- 연관 배열을 정의할 때 자료형이 TABLE인 변수를 다음과 같이 작성한다.

```sql
// 기본 형식
TYPE 연관 배열 이름 IS TABLE OF 자료형[NOT NULL]
INDEX BY 인덱스형;
// 1. 연관 배열 이름
// 2. 자료형 [NOT NULL]
// 3. 인덱스형
```

| 번호 | 설명 |
|:---:|:---------:|
| 1. | 작성할 연관 배열 이름을 지정합니다. |
| 2. | 연관 배열 열에 사용할 자료형을 지정합니다. 이 자료형에는 VARCHAR2, DATE, NUMBER와 같은 단일 자료형을 지정할 수 있고 %TYPE, %ROWTYPE 같은 참조 자료형도 사용할 수 있다. NOT NULL 옵션을 사용할 수 있으며 생략 가능하다. |
| 3. | 키로 사용할 인덱스의 자료형을 지정합니다. BINARY_INTEGER, PLS_INTEGER 같은 정수 또는 VARCHAR2 같은 문자 자료형도 사용할 수 있습니다. |

<br/>

> 컬렉션 메서드

- 오라클에서는 켈렉션 사용상의 편의를 위해 몇 가지 서브프로그램을 제공하고 있다. 이를 컬렉션 메서드라고 한다.

- 컬렉션 메서드는 컬렉션과 관련된 다양한 정보 조회 기능을 제공한다. 이와 더불어 컬렉션 내의 데이터 삭제나 컬렉션 크기 조절을 위한 특정 조작도 가능하다.

| 메서드 | 설명 |
|:------:|:-----------------:|
| EXISTS(n) | 컬렉션에서 n인덱스의 데이터 존재 여부를 true/false로 반환합니다. |
| COUNT | 컬렉션에 포함되어 있는 요소 개수를 반환합니다. |
| LIMIT | 현재 컬렉션의 최대 크기를 반환합니다. 최대 크기가 없으면 NULL을 반환합니다. |
| FIRST | 컬렉션의 첫 번째 인덱스 번호를 반환합니다. |
| LAST | 컬렉션의 마지막 인덱스 번호를 반환합니다. |
| PRIOR(n) | 컬렉션에서 n인덱스 바로 앞 인덱스 값을 반환합니다. 대상 인덱스 값이 존재하지 않는다면 NULL을 반환합니다. |
| NEXT(n) | 컬렉션에서 n인덱스 바로 다음 인덱스 값을 반환합니다. 대상 인덱스 값이 존재하지 않는다면 NULL을 반환합니다. |
| DELETE | 컬렉션에 저장된 요소를 지우는 데 사용합니다. 다음 세 가지 방식으로 사용합니다.<br/> • DELETE : 컬렉션에 저장되어 있는 모든 요소를 삭제합니다.<br/> • DELETE(n) : n인덱스의 컬렉션 요소를 삭제합니다.<br/> • DELETE(n,m) : n인덱스부터 m인덱스까지 요소를 삭제합니다. |
| EXTEND | 컬렉션 크기를 증가시킵니다. 연관 배열을 제외한 중첩 테이블과 VARRAY에서 사용합니다. |
| TRIM | 컬렉션 크기를 감소시킵니다. 연관 배열을 제외한 중첩 테이블과 VARRAY에서 사용합니다. |

- 컬렉션 메서드는 컬렉션형으로 선언한 변수에 마침표(.)와 함께 작성하여 사용할 수 있다.

---
