# error: Your local changes to the following files would be overwritten by merge:

## ✏️발생배경

1. 반응형 구현을 완료한 뒤 깃허브에 push하고 merge를 하기 위해
```
git commit -m "[add] 반응형 레이아웃 구현 완료"
```
로 커밋을 시도하였지만 

```
use "git pull" to merge the remote branch into yours
```
`merge`하기 위해서는 `pull`을 먼저 받으라는 메시지를 받게 되었습니다.

<br>

2. 실패의 이유가 pull을 하지 않아서 그렇구나 라고 생각하여
```
git pull origin main
```
으로 pull도 시도하였지만

```
error: Your local changes to the following files would be overwritten by merge:
        안내사항/index.html
Please commit your changes or stash them before you merge.
```
다음과 같은 에러가 등장하며 `pull`또한 되지 않았습니다.

<br>

## ✏️발생원인
터미널을 정독해본 뒤 아래의 메시지를 발견하게 되었습니다.
```
Please commit your changes or stash them before you merge.
```
해석해보니 합병하기 전, 나의 커밋을 바꾸거나 `stash`를 하라는 말이었습니다.

`stash`가 무엇인지 몰라 위의 메시지를 검색해보니 
**말 그대로 merge를 했을 때 내가 작업했던 작업물이 날아갈 수 있기때문에 에러가 났던 것이었습니다.**

<br>

## ✏️해결
저는 **내가 한 작업들을 임시로 저장해둘 수 있는 기능인 git stash**로 이 에러를 해결하였습니다.

1. `git stash`로 나의 변경사항을 임시저장합니다.
2. `git pull origin main`으로 기존의 변경사항을 끌어옵니다.
3. `git stash pop`으로 임시저장했던 나의 변경사항을 가져옵니다.

<br>

## 💡배운점
전부터 `git pull`을 했을 때 내가 수정했던 부분과 겹치면 어떻게 하지 하는 생각이 들었었는데 이렇게 `conflict`가 발생할 경우 `git stash`로 임시저장을 하면 된다는 사실을 알게되었습니다.

프로젝트를 진행하며 `conflict`가 날 수 있는 상황이 충분히 있을 것이고 그에 대처할 수 있는 경험을 쌓을 수 있어 좋은 경험이었다고 생각합니다.