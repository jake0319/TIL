# STEP1
- mysql서버 중지시켜주기   
`시스템환경설정(사과아이콘) - Mysql - Stop MySQL server `


# STEP2
- A터미널에서 서버종료 후 안전모드 실행하기  
`sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables`  
// password 입력 요구시 mac비밀번호
  
- B터미널 오픈 후 mysql 접속
`sudo /usr/local/mysql/bin/mysql -u root`  
//접속완료

# STEP3 
- root 비밀번호 삭제해주기 (null)
```
UPDATE mysql.user SET authentication_string=null WHERE User='root';

FLUSH PRIVILEGES;

exit;
```

# STEP4
- 다시 mysql에 접속 후 패스워드를 변경해줍니다.(바꿀 비밀번호에 new_password입력)
```
sudo /usr/local/mysql/bin/mysql -u root

ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '바꿀 비밀번호';

FLUSH PRIVILEGES;

exit;
```
# STEP5
- mysql 재시작 
`sudo /usr/local/mysql/support-files/mysql.server restart`
// 재시작하게되면 A터미널에서 안전모드를 실행했던 것도 종료됩니다.

`FIN`

<br>
<br>

### 출처: 
[mysql 8.0 비밀번호 분실시_패스워드 변경방법](https://kwangkyun-world.tistory.com/entry/mysql-80-%ED%8C%A8%EC%8A%A4%EC%9B%8C%EB%93%9C-%EB%B3%80%EA%B2%BD-%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8-%EC%B4%88%EA%B8%B0%ED%99%94-mac#recentComments)
