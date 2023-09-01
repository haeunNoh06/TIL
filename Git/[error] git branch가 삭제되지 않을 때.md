# git branch 삭제가 안 될 경우 발생한 에러

## ✏️발생배경
기본 브랜치가 master인 리포지토리의 기본 브랜치를 main으로 변경하는 작업을 하고 있었는데 master 브랜치를 삭제하려고 하니 아래와 같은 에러 메시지가 뜨게 되었습니다.
```
error: Cannot delete branch 'master' checked out at
```

<br>

## ✏️발생원인
발생원인을 확인하려고 지금 있는 브랜치 목록을 아래의 명령어를 통해 살펴보니
```
git branch -a
```
![image](https://github.com/haeunNoh06/Watch-Out-Bees/assets/113562640/80e3d9a5-cd99-47d3-a9cc-b921d06d404e)
위와 같이 현재 브랜치가 master임을 확인할 수 있었습니다.

아, 현재 브랜치가 master이기 때문에 삭제가 불가능했던 거구나!

<br>

## ✏️해결과정
따라서 아래의 명령어로 main 브랜치로 변경해준 뒤
```
git checkout main
```

```
git branch -d master
```
master 브랜치를 삭제해주었더니
![image](https://github.com/haeunNoh06/Watch-Out-Bees/assets/113562640/a32169b3-80af-4dd6-88a5-a5f1347b3d41)
위와 같이 main 브랜치만 잘 남게 되었습니다.

<br>

## 💡배운점
바보같이 현재 브랜치가 master인데 master를 지우려 했던 실수를 했습니다.
다음부터는 현재 브랜치를 잘 알아보고 브랜치를 수정하던 삭제하는 작업을 해야겠어요.

잘못해서 변경하면 안 될 브랜치를 변경할 수도 있으니 말입니다.