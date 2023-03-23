# DML 정리 (INSERT, UPDATE, DELETE, SELECT), (산술/합성 연산자)

**INSERT 문**
* 각 Attribute에 대해 지정된 값을 가지고 Table에 새로 행(Row)을 추가할때 사용
* Inserting a new row to an existing table with attributes. (The newly inserted attribute values must follow the Table's order by which was declared by CREATE TABLE)

```
/* TYPE 1 */
INSERT INTO <table name> [attribute names]
VALUES [values]
```
```
/* TYPE 2 */
INSERT INTO <table name> [attribute names]
select statement
```

<EXAMPLE 1>

Given an empty generated table
```
/* Table name = Player (NAME AND AGE IS NOT NULL)*/
[NAME][AGE][GENDER][HEIGHT][WEIGHT]
```
Code:
```
INSERT INTO Player /* 생략 */
VALUES ('SungKyuLee', 22, 'M', 174, 65)
```
Result:
```
/* Table name = Player */
[   NAME   ][AGE][GENDER][HEIGHT][WEIGHT]
[SungKyuLee][22 ][M     ][174   ][65    ]
```

<EXAMPLE 2>

Code:
```
INSERT INTO Player (NAME, AGE, GENDER)
VALUES ('SungKyuLee', 22, 'M')
```
Result:
```
/* Table name = Player */
[   NAME   ][AGE][GENDER][HEIGHT][WEIGHT]
[SungKyuLee][22 ][M     ][null  ][null  ]
```
Height와 Weight은 기본 Default value가 명시되지 않았고, Insert문에서 Attribute를 명시하지 않았기 때문에 값이 없다. 단 Height와 Weight이 NOT NULL 상태이면서 Default value도 없으면 위 코드는 에러가 발생한다.

**UPDATE 문**
* 선택된 하나 이상의 튜플에서 Attribute 값들을 수정하기 위해 사용
* Used to change the attributes in one or more then one Tuples

```
/* TYPE 1 */
UPDATE <table name>
SET <attribute name> = <value> , ...
WHERE <selection condition>
```

**DELETE 문**
* 한 Table에서 튜플을 삭제하기 위해 사용
* Delete Tuples in a Table

```
/* TYPE 1 */
DELETE FROM <table name>
WHERE <selection condition>
```

**SELECT 문**
* Table에서 정보를 Select하기 위해 사용
* Select 문장은 Select절, From절, Where절과 같은 3가지 절로 구성
* (SELECT *) = 모든 Attribute를 select | (SELECT distinct) = 중복된 튜플 삭제하고 하나만 출력 (기본은 SELECT ALL, 하지만 ALL은 생략)

```
/* TYPE 1 */
SELECT <attribute name>
FROM <table name>
WHERE <conditions>
```

<EXAMPLE 1> Simple Search

Given an empty generated table
```
/* Table name = Player */
[accountID][firstName][lastName][street          ][city       ][State][zipcode][balance]
[111      ][Jane     ][Saw     ][123 Main st     ][Apopka     ][FL   ][34331  ][0.00   ]
[444      ][Jane     ][Doe     ][Cawthon Dorm 142][Tallahassee][FL   ][32306  ][10.55  ]
```
Code:
```
SELECT *
FROM Player
WHERE lastName = 'Doe'
```
Result:
```
/* Table name = Player */
[accountID][firstName][lastName][street          ][city       ][State][zipcode][balance]
[444      ][Jane     ][Doe     ][Cawthon Dorm 142][Tallahassee][FL   ][32306  ][10.55  ]
```

<EXAMPLE 2> Using Boolean Expressions

Code:
```
SELECT *
FROM Player
WHERE city = 'Apopka' and zipcode < 24000
```
Result:
```
/* Table name = Player */
[accountID][firstName][lastName][street          ][city       ][State][zipcode][balance]
```

<EXAMPLE 3> SELECT distinct

Code:
```
SELECT distinct lastName
FROM Player
```
Result:
```
/* Table name = Player */
[lastName]
[Saw     ]
[Doe     ]
```

**SELECT문에서 부분 문자열 패턴 비교**
* Like 비교 연산자
  - % = 임의의 가변 길이 문자열을 대표
  - (_) = 한 문자를 대표
  - 모든 패턴들은 대소문자 구분함
  - not like = 일치하지 않는 문자열 대표

<EXAMPLE 1> %

Code:
```
SELECT *
FROM Movie
WHERE genre like '%comedy' 
```
Result:
```
comedy로 끝나는 모든 genre 출력. (ex. action comedy, romance comedy).
반대로 'action%' 할시, action으로 시작하는 모든 genre 출력
```

<EXAMPLE 2> (_)

Code:
```
SELECT *
FROM Employee
WHERE ssn like '_ _ _ - 444 - _ _ _'
```
Result:
```
Employee Table에서 SSN attribute중 저 형식을 맞는 모든 직원을 검색하고 출력.
(_) 하나 = 문자열 하나
```

**산술 적용**
* as (aliasing name) = 질의 결과 내에 있는 attribute에 새로운 이름을 부여 (안써도 Default 값)

Code:
```
SELECT lastName as "성 이름", firstName as 이름
FROM Employee
```
Result:
```
[성 이름][이름]

Attribute 안에 공백을 넣을려면 코드에 "" 사용
```

**정렬**
* 기본 값은 오름차순 정렬 (asc)

```
/* TYPE 1 */
SELECT *
FROM Player
order by lastName asc, firstName desc
```
