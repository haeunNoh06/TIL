# MySQL 계정 생성 및 권한 부여

## 1. 사용 방법
1. 사용할 데이터 베이스 생성
2. 계정 생성 (id, password)
3. 계정에 사용권한 부여 (CRUD 쿼리 권한)

<br>

## 2. mysql 접속 방법
```mysql
mysql -u아이디 -p비밀번호 사용할데이터베이스이름
```
- ex. `mysql -uroot -p1111 mysql`

이렇게 `root`계정으로 접속을 완료했다.
유저 또는 데이터 베이스를 생성하기 위한 쿼리문은 `mysql`에 접속해야 하기 때문에 꼭 `root`계정으로 접속한 뒤 명령어를 실행해야 한다.

<br>

## 3. 유저(계정) 생성하기
```mysql
CREATE USER '생성할유저이름'@localhost IDENTIFIED BY '접속비밀번호';
```
- ex. `CREATE USER 'test'@localhost IDENTIFIED BY '111111';`

이제 `test`유저에 `111111`이라는 비밀번호로 접속할 수 있다.

<br>

## 4. 계정에 CRUD 권한 부여하기
Create, Read, Update, Delete를 할 수 있도록 권한을 부여한다.

### 4.1. 권한 부여하기
```mysql
grant all privileges on 데이터베이스명.* to 계정아이디@'호스트' with grant option;
```
- ex. `grant all privileges on test.* to test@'localhost' with grant option;`

<br>

### 4.1. 부여한 권한 최종적으로 적용시키기
```mysql
flush privileges;
```

<br>

### 4.2. 권한 확인하기
```mysql
SHOW GRANTS FOR '계정아이디'@'호스트';
```
- ex. ` SHOW GRANTS FOR 'test'@'localhost';`

<br>

## 5. 계정 삭제하기
```mysql
DROP USER '계정아이디'@'호스트';
```
- ex. `drop user 'test'@'localhost;`

<br>

## 💡배운점
php와 mysql을 연동하기 위해 간단히 알아보았다.
수업시간에 다 배웠는데도 불구하고 mysql 명령어를 사용하기 위해서는 **root 계정에 접속한 뒤에 명령어를 작성해야 한다**는 기본을 빼먹어서 애먹었다ㅜㅜ

덕분에 mysql 계정을 생성하고 데이터베이스를 생성하는 것은 완벽하게 알게 된 것 같다.