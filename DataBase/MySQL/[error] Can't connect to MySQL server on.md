# ERROR 2003: Can't connect to MySQL server on 'localhost:3306'

## ✏️발생배경
Java GUI 프로젝트를 진행하려 로컬에서 mysql의 root 계정으로
```sql
mysql -u root -p
```
로 접속을 시도하고 비밀번호를 입력하였을 때, 다음과 같은 에러 메시지가 발생하게 되었습니다.

> ERROR 2003: Can't connect to MySQL server on 'localhost:3306'

- localhost의 3306포트에 있는 mysql 서버와 연결할 수 없다.

<br>

## ✏️발생원인
해당 오류를 구글링해보니 크게 세 가지의 해결 방법이 존재하였습니다.

1. 비밀 번호가 틀린 경우 - 보통 이 문제가 가장 많은 것 같았습니다.
2. MySQL서버가 작동이 되지 않고 있던 경우
3. MySQL installer에서 비밀번호 업데이트를 해주지 않은 경우

<br>

### 1. 비밀번호가 틀린 경우
저도 처음에는 이 에러인줄 알았지만 다른 문제가 있었습니다.
하지만 나중에 또 같은 에러 발생을 막기 위해 기록해두겠습니다.

1. MySQL이 설치된 경로에서 bin폴더에 들어갑니다.
- ex) C:\Program Files\MySQL\MySQL Server 8.0\bin
- 만약 **Path**를 설정했다면 cmd창을 킨 뒤 다른 경로 설정 없이 바로 2번을 입력합니다.
2. `mysqld --skip -grant`명령을 실행하여 암호 없이 MySQL에 접속할 수 있도록 조치합니다.
3. cmd창을 새로 띄운 뒤 `mysql -u root -p`명령어로 MySQL에 접속합니다.
4. `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '변경할비밀번호';`로 root유저의 비밀번호를 재설정합니다.
5. `flush privileges;`로 변경된 사항을 저장합니다.
6. 변경한 비밀번호로 다시 로그인하면 MySQL Server에 접속이 됩니다.

<br>

### 2. MySQL 서버가 작동이 되지 않고 있던 경우

1. 로컬에서 **서비스**라는 앱을 실행합니다.
2. 들어가자마자 보이는 화면에서 **MySQL**을 찾습니다.
3. 찾은 MySQL의 상태가 **실행**인지를 확인한 뒤, 실행되고 있지않다면 실행시켜줍니다.

<br>

## ✏️문제해결
하지만 저는 위의 두 해결방법에서는 해당사항이 없었습니다.

혹시나 `Connection`에 문제가 있나 싶어 `workbench`를 실행하고 `root`계정의 `connection`을 확인하였더니 다음과 같은 에러메시지가 나며 `connection`조차 되지 않는 모습을 확인할 수 있었습니다.

> failed to connect to mysql at 127.0.0.1:3306

검색을 해보니 connection이 되지 않는 이유 중 하나는 MySQL 서버에 접속조차 안 되는 것이 있다고 하였습니다.

최종적으로는 마지막 세 번째 방법에서 이 문제를 해결할 수 있었습니다.

1. **MySQL installer**를 들어갑니다.
2. **MySQL Server**의 오른쪽에 있는 **Reconfigure**을 누른 뒤 전부 next를 누릅니다.
3. 비밀번호를 체크하는 화면이 나왔을 때, 현재 알고있는 비밀번호를 입력하여 업데이트해줍니다.
4. 똑같이 next를 누른 뒤 종료합니다.
5. cmd창을 새로 띄운 뒤 `mysql -u root -p`를 입력합니다.
6. 3번에서 입력했던 비밀번호를 입력합니다.

이렇게 무사히 mysql에 접속할 수 있게 되었습니다.

<br>

## 💡배운점
아마 이 전의 오류를 해결하기 위해 비밀번호를 한 번 바꾸었었는데, 그 변경사항이 잘 저장되지 않아, 즉 업데이트가 되지 않아 비밀번호가 맞지 않았던 것 같습니다.

이 문제를 해결하는 과정에서 해당 오류가 한 상황에서만 일어나지 않고 다양하게 일어난다는 것을 알게 되었습니다.

앞으로 이 에러를 만나게 되더라도 전부 유연하게 대처할 수 있도록 많은 경우의 수를 알아야겠습니다.

<br>

### 참고자료
> https://velog.io/@qowl880/mysql-%EC%97%B0%EA%B2%B0-%EC%8B%A4%ED%8C%A8-%EC%98%A4%EB%A5%98failed-to-connect-to-mysql-at-localhost-3306-with-user-root
> https://codejinjinh.tistory.com/159
> https://hoon93.tistory.com/9

<br>

***