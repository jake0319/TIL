# Database 

## 1. db종류 
RDBMS 관계형데이터베이스: 정확도가 중요할 때
Document database :수시로 잦은 입출력이 필요한 경우(실시간sns등..)

<br>

## 2. mysql및 환경설정

**(요약)**
- 1. mysql community Mac검색 후 m1버전설치(os 버전12,13확인)
- 2. root계정 비밀번호 설정
- 3. mysql 환경설정 후 mysql 서버띄우기
- 4. DBeaver(workbench같은 gui툴)설치후 열기


### Problem 
>일반적으로 Mac에서 모든 설치 파일은(ex: MySQL)
`해당 파일이 설치된 폴더로 이동해야 파일을 실행할 수 있다.`
하지만 터미널 상에서 매번 해당 파일이 설치된 폴더로 cd 해서 이동하는 건 너무 `비효율적`이다.

### 사용중인 쉘 확인하기 
- iterm터미널을 열면 상단에 `-zsh` 확인 
- `ECHO $SHELL` : `/bin/zsh` ( zsh쉘 사용중임을 확인)

### vim에디터 또는 vscode로 환경변수 설정하기 
- `vi ~/.zshrc 또는 code ~/.zshrc` (환경변수설정폴더)
- export PATH="$PATH:/usr/local/mysql/bin" 추가 후 저장 
    - 이제 모든 경로에서 `usr/local/mysql/bin폴더` 속 파일에 접근이 가능하다.
    - 터미널상에서 mysql 명령어 사용이 가능 (`>mysql -u root -p`)
- 환경변수 설정했으면 터미널끄고 재시작

### root유저로 mysql서버 띄워주기
`mysql -u root -p`
- 터미널에 위 명령어 입력하면 root 유저로 로그인이 가능합니다.
- 이거 입력해둬야 앞으로 MySQL 조작이 가능합니다.
- 이제 Dbeaver열어서 db조작하기 

<br>

[참고사이트: Mac - Path 추가하기 (+bash쉘, zsh쉘, echo$PATH, vi 편집기)](https://lovelydiary.tistory.com/309)

<br>


## 3. 테이블만들기 & 데이터타입 

Database
-> 데이터베이스명(폴더명)
-> 테이블(일종의 엑셀파일)

### 컬럼 만들기
- (ex 인덱스,상품명,가격,카테고리 등...) n개만들기 -> save & persist -> data탭에서  컬럼확인 
-> 로우만들고 데이터타입 및 데이터  value 채워주기


<br>


## 4. 데이터 출력하고 정렬하는법(SELECT,ORDER BY)
- 테이블작성 후 데이터베이스명 클릭후 SQL편집기로 이동,sql문 작성후 스크립트실행
( sql문법은 대소문자 구분 없음, 글로벌세팅가능)

### 4.1 `SELECT`: 데이터 뽑고싶을 때 
- `select * from 테이블`: 테이블에 있는 모든(*)데이터 출력
// 간혹 안되는경우 데이터베이스명.테이블명

### 4.1.1 `특정` 컬럼만 뽑고싶을 때 
- `select 컬럼명 from 테이블명`

### 4.2 정렬기준 명시 ORDER BY `기준 컬럼명`
- 기준명 ASC(ascending: 오름차순 디폴트)
// ABCD, 1234 ...
- 기준명 DSC
// DCBA, 4321 ...
- 기준컬럼명 대신 ORDER BY N(몇번째 컬럼인지) 숫자로도 기준컬럼 명시가능합니다.

### 4.2.1 또 다른 기준으로 2차 정렬도 가능 
- ex) 기준명 ASC, 2차기준명 DESC
`SELECT * FROM product ORDEY BY 상품명 ASC, 가격 DESC
// 상품명 오름차순 정렬 후, 동일 상품명의 순서에 대해 가격기준 내림차순 (2차)정렬해줌.

<br>

## 5. 원하는 `행`만 필터링 하고 싶을 때(WHERE)
`WHERE 조건식 `
- WHERE 가격 > 5000 `(>= , <= , !=  등... 가능)`
- WHERE 가격 BETWEEN 5000 and 8000

## 6. WHERE 뒤에 조건식 여러개 쓰려면
- 조건식이 2개 이상 필요한 경우 AND(교집합),OR(합집합),NOT(!)을 붙여서 연결가능

```sql
SELECT * FROM product 
WHERE NOT 카테고리 = '가구'

SELECT * FROM product 
WHERE (카테고리 = '가구' OR 카테고리 = '옷') AND 가격 = 5000
```
- 칼럼명 ='value' , 해당 칼럼명이 아닌 칼럼만 출력
- 괄호가 있으면 괄호 안의 조건식을 먼저 처리 

### IN 문법
```SELECT * FROM product 
WHERE 카테고리 = '신발' OR 카테고리 = '가전' OR 카테고리 = '식품'

SELECT * FROM product 
WHERE 카테고리 IN ('신발', '가전, '식품')
```
- OR이 여러개 쓰인다면 IN으로 대체 가능 (행필터링)

>오늘의 결론 : 

1. SELECT FROM 뒤에 WHERE 조건식을 붙여서 필터링할 수 있고

2. 조건식란엔 > / < / = / != / >= / <= 전부 이용가능하고 

3. 조건식 여러개가 필요하면 AND OR 이런걸로 이어붙일 수 있고 

4. 괄호로 AND OR 사용한 부분을 묶을 수도 있고

5. OR 조건식 여러개 필요하면 IN () 사용해도 될 때가 있습니다.

<br>

## 7. `LIKE, % , _` 연산자로 검색기능 만들기
`WHERE column LIKE 'Value'`

ex)`WHERE 상품명 LIKE '%소파%'`
- 상품명중 '소파' 글짜가 들어간 모든 행을 찾아줍니다.
- %는 아무문자를 뜻합니다. 
- '%소파' : 소파로 끝나는 글자가 들어간 행을 모두 찾아줌
- '소파%' : 소파로 시작하는 글짜가 들어간 모든 행을 찾아줌 
- 글자수제한이 있는 아무문자 연산자 `%(0~무제한)` `_(아무글자 1개)`


응용예) 상품명에 '소파'가 들어있는데 '나무'는 들어있지 않은 모든 상품을 검색해봅시다. 
```sql
SELECT * FROM newtable WHERE 상품명 LIKE '%소파%' AND NOT 상품명 LIKE '%나무%'
```


## 8. MIN,MAX,AVG,SUM 집계함수로 통계내기 
`데이터분석 = 의미찾기 = 가장쉬운방법 ~ 통계내기`
- 집계함수를 사용하여해서 `특정 컬럼`의 합계,평균,최댓값등을 구해줌
>
1. 최대,최소값 MIN(),MAX		
// SELECT MAX(사용금액) FROM card 
// SELECT MIN(사용금액) FROM card 

2. 평균내려면 AVG()
// SELECT AVG(사용금액) FROM card 

3.. 합 SUM()
// SELECT SUM(사용금액) FROM card

4. 행의 갯수 
// SELECT COUNT(사용금액) FROM card
SELECT COUNT

### 응용1) 컬럼명을 바꾸고싶다면 AS
// SELECT MAX(사용금액) AS 최대사용금액 FROM card
### 응용2) 필터링 후에 통계내기
- 고객등급(컬럼)이 vip(값)인 사람의 평균 사용금액은?
// SELECT AVG(사용금액) FROM card WHERE 고객등급 = 'vip'
### 응용3) DISTINCT키워드로 중복제거 
// SELECT DISTINCT 연체횟수 FROM card
// SELECT AVG(DISTINCT 연체횟수) From card 로 섞어쓰기도 가능


## 9. 컬럼 출력시 사칙연산 넣기 & 문자 다루는 함수




## 11. 서브쿼리 (쿼리 in 쿼리)
- 1. 데이터(값) 대신 서브쿼리 넣기 가능
- 2. 1개의 데아터만 뱉는 쿼리문만 서브쿼리 역할 가능
- 3. 소괄호 꼭 넣기
//('문자,숫자 값'  대신에 괄호치고 서브쿼리문 넣으면 됨)
- 4. (예외) IN()안에서는 여러 값을 반환하는 서브쿼리 사용가능

```sql

SELECT 사용금액 FROM card
WHERE 고객명 IN('Pristine','George','Amy')

SELECT * FROM card 
WHERE 고객명 IN(SELECT 이름 FROM blacklist)
```
// IN('Pristine','George','Amy')
// JOIN으로 대체가능.


Q1) (고객등급이 패밀리인 사람들의 평균 연체횟수)보다 (연체횟수)가 높은 사람?

>SELECT AVG(연체횟수) FROM card WHERE 고객등급 = '패밀리'
//6.25
SELECT COUNT(*) FROM card WHERE 연체횟수 > 6.25
// COUNT : 모든칼럼(=특정칼럼)의 행 갯수
// 모든 칼럼중 연체횟수 >6.25인 행의 수 

Q2) 테이블 출력:  고객명, 사용금액 ,사용금액 - 사용금액평균(as diff)
`SELECT 고객명,사용금액, 사용금액 - (SELECT AVG(사용금액) FROM card )AS DIFF FROM card`



<br>



## 12. 그룹지어 통계낼 땐 GROUP BY
```sql
SELECT * FROM card WHERE 고객등급 = 'vip'
SELECT * FROM card WHERE 고객등급 = '패밀리'
SELECT * FROM card WHERE 고객등급 = '로열'

...///

SELECT 고객등급, avg(사용금액), MAX(사용금액)  FROM card GROUP BY 고객등급
// 고객등급컬럼에서 자동 그룹핑을 해서 (고객등급, avg(사용금액)을 출력해줌
```
- 1. GROUP BY 컬럼명하면, 컬럼의 같은 값끼리 모아줌
//주로 카테고리 컬럼에 쓰임 (컬럼의 값에 중복값들로 이루어져 그룹핑이 되는경우)
- 2. 집계함수(AVG,MAX,...) 사용
- 3. GROUP BY에 필터링 적용시 WHERE이 아니라 'HAVING'
```sql
SELECT 고객등급, MAX(사용금액) FROM card
GROUP BY 고객등급 HAVING 고객등급 = 'vip', 고객등급 = '패밀리'
```
> WHERE은 SELECT FROM 결과 필터링
HAVING은 GROUP BY 결과 필터링

> SELECT / FROM / WHERE / GROUP BY / HAVING(group by + having 필터링 조건문,where 대체) / ORDER BY(정렬) 순서로 작성

Q1) 연체 횟수마다 몇명이 있는지
SELECT 연체횟수, COUNT(연체횟수) AS 인원수  FROM card GROUP BY 연체횟수 ORDER BY 연체횟수
<br>


## 13. 중요한 IF/CASE 문법

### 경우에 따라다른 값을 뱉으려면 IF문(양자택일)
- IF(조건식, 참이면 뱉을 값, 거짓이면 뱉을 값)
- `SELECT IF(1 + 2 = 3, 'aaa','bbb')`
- 단점, 참-거짓일 때 반환하는 양자택일문제 발생


### 케이스가 3개이상 CASE문
```sql
SELECT 사용금액,
CASE 
	WHEN 사용금액 >= 200000 THEN '우수'
	WHEN 사용금액 >= 100000 AND 사용금액 < 200000 THEN '준수'
	ELSE '그지'
//위에있는 값들이 전부 아니라면 '이거'남겨주세요
END AS 평가 
FROM card;
```

응용1) 고객등급에 따른 점수 부여(3,2,1) 의 합 
```sql
SELECT SUM(
CASE 
	WHEN 고객등급 = 'vip' THEN 3
	WHEN 고객등급 = '로열' THEN 2
	ELSE 1
END
) AS 평가 
FROM card;

```
- SUM은 기본적으로 SUM(3)하면 테이블의 행 갯수만큼(15) 3*15 =45리턴
- 행 갯수만큼 sql문을 반복하는데.. case문은 평가값이므로 SUM()함수에 들어갈 수 있음.

응용2) 고객등급이 바뀌는 고객들만 추려서 출력하기
```sql
SELECT 고객명, 사용금액 , 고객등급
FROM card
WHERE 고객등급 != CASE 
	WHEN 사용금액 >= 300000 THEN 'vip'
	WHEN 사용금액 >= 200000 AND 사용금액<300000 THEN '로열'
	ELSE '패밀리'
END
```
- 행 필터링, (CASE문 리턴값과 고객등급이 전후가 일치하지않는 행만 필터링)


<br>


요약)
- SELECT 컬럼명 FROM 테이블명:  컬럼 선택
- ORDER BY 컬럼명(넘버): 정렬기준 선택
- WHERE 조건식: 원하는 행만 필터링
- WHERE 조건식에 사용가능한 AND,OR,NOT,IN
- 검색기능은 WHERE 컬럼 LIKE '%문자열%'
- 









