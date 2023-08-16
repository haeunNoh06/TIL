# Firebase 웹 호스팅 생성, 배포하기

## 0. firebase를 사용할 폴더 만들기
```
mkdir 폴더명
```
- **mkdir** : make directory

저는 `minto-deploy`라는 폴더를 만들어주고 `cd`명령어로 해당 폴더 안으로 들어가주었습니다.

<br>

## 1. firebase hosting 설정하기
### 1.1. firebase CLI 설치
```
npm install -g firebase-tools
```
- **CLI** 설치 명령어
- Firebase 호스팅으로 사이트를 호스팅하려면 **Firebase CLI**(명령줄 도구) 가 필요합니다.

<br>

## 2. 프로젝트 초기화 (login, init)

### 2.1. firebase login
```
firebase login
```
이미 로그인을 했었다면 `Already logged`가 뜨지만 처음 로그인을 하면 **성공**이라는 메시지가 뜰 겁니다.

<br>

### 2.2. firebase init
```
firebase init
```
- **Are you ready to proceed?**에는 Y

- **Hosting** 클릭
    - 이 때, **space바**를 누르면 \*이 찍히고, **enter**를 누르면 선택이 됩니다.


- **Use an existing project**를 **enter키**를 눌러 선택해주세요.

- firebase에서 만들었던 나의 프로젝트 이름을 선택하고 **enter키**를 눌러주세요.

- **public**으로 작성하고,

- **N**을 작성하고,

- **N**을 작성하면,

firebase 초기화가 완료!

<br>

## 3. firebase 배포 및 확인

### 3.1. firebase 배포 전 테스트
```
firebase serve
```

해당 명령어를 작성하면 로컬에서 호스팅이 잘 되었는지를 먼저 테스트 해볼 수 있습니다.

`localhost:5000`에 접속하면 호스팅을 테스트해볼 수 있는데요,
아직 `index.html`을 수정하지 않았기 때문에 기본 화면이 나타날 것입니다.!

<br>

### 3.2. firebase 배포하기
드디어 최종 배포를 할 수 있습니다!

```
firebase deploy
```
해당 명령어를 입력하면 호스팅이 완료되었다고 하며 배포 링크가 뜨게 됩니다.
저는 `https://minto2023-2e5fe.web.app`이 나왔네요.
해당 링크로 들어가면 `serve`와 동일한 페이지가 나오게 될텐데 
`index.html`을 수정하면 그에 따른 결과가 해당 링크에 나오게 됩니다.

<br>

> 어? 저는 `index.html`을 수정해도 수정하기 전과 똑같은 화면인데요??

firebase의 **빌드 > Hosting** 에 들어갔을 때 배포 도메인이 있다면 배포는 완료된 겁니다.
다만 아직 업데이트가 되지 않았을 수도 있으니 다음 날 다시 한 번 들어가보시면 아마 변경되었을 거에요.

<br>

## 💡배운점
이렇게 firebase로 호스팅을 해봤는데 배포를 너무나도 간단하게 할 수 있어서 좋았고 앞으로 많이 쓸 수 있겠다는 생각이 들었습니다.

그리고 절차에 따라 진행했음에도 불구하고 배포링크에서 변경된 사이트를 볼 수 없었던 오류가 나왔었는데,
그저 업데이트가 느렸던 것이라는 사실을 깨닫고 앞으로 이러한 오류가 나더라도 의연하게 대처할 수 있을 것 같습니다.

여러모로 유익했던 배포였습니다!