# Process 'command finished with non-zero exit value 1 에러 해결 
## ✏️1. 발생배경
IntelliJ, Gradle 환경에서 api를 개발하던 중 아래와 같은 에러가 발생하게 되었습니다.
```
Process 'command finished with non-zero exit value 1
```

![](https://velog.velcdn.com/images/1109_haeun/post/13ca47a4-e788-4ce1-95d8-5bf560d17a10/image.png)

## ✏️2. 발생원인
> 빌드 환경이 IntelliJ IDEA가 아닌 Gradle로 되어 있어 해당 오류가 발생했던 것이었습니다

![](https://velog.velcdn.com/images/1109_haeun/post/7ed489c6-ee5e-4297-9074-a98b9d2bf1c3/image.png)


## ✏️3. 문제해결
해당 오류를 해결하기 위해서는 빌드 환경을 IntelliJ IDEA로 변경해주어야 합니다.

1. `File > Settings > Build, Excution, Deployment > Build Tools > Gradle`로 들어갑니다.
2. `Build and run using`과 `Run tests using`을 `Gradle (Default) -> IntelliJ IDEA`로 바꿔줍니다.

![](https://velog.velcdn.com/images/1109_haeun/post/0fd985f0-3858-49d5-8c0d-5a47b6d802eb/image.png)<br>
변경 후에는 정상적으로 실행되는 것을 확인할 수 있었습니다. <br>
![](https://velog.velcdn.com/images/1109_haeun/post/2580e5a6-03cd-4dc0-bb3f-f943d11c05dd/image.png)

## 4. 💡배운점
초기 세팅에서 `Gradle`을 `IntelliJ IDEA`로 변경하는 것을 까먹었을 때 많이 발생했을 것 같은데 초기에 알아차려서 후에도 잘 해결할 수 있을 것 같습니다.