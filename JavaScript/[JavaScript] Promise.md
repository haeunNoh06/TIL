
---
# 1. Promise란?

> Promise: 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과값을 나타내는 객체

`promise`는 **비동기 연산이 종료된 이후**에 결과값과 실패 사유를 처리하기 위한 처리기를 연결할 수 있습니다.

다만 최종 결과를 반환하는 것이 아니라 미래의 어떤 지점의 결과를 제공하겠다는 **약속**을 반환합니다.

<br>

---
## 1.1. Promise의 상태
promise는 총 세 가지의 상태를 가집니다.

- **대기 (pending)** : 이행하지도, 거부하지도 않은 초기 상태
- **이행 (fulfilled)** : 연산이 성공적으로 완료됨.
- **거부 (rejected)** : 연산이 실패함.

쉽게 말하면 처음에는 **대기**상태에 있다가 연산의 성공 여부에 따라서 성공이라면 **이행**을, 실패했다면 **거부**의 상태를 가지는 것입니다.

<br>

---
## 1.2. Promise 호출 과정
promise 호출 과정은 다음과 같습니다.

1. 비동기 함수 내에서 `Promise` 객체를 생성한 뒤, 그 내부에서 비동기 처리를 구현합니다.
    - 만약 비동기 처리에 성공하면 `resolve` 메서드를 호출합니다.
2. `resolve`메서드가 호출되었을 때, `resolve`메서드의 인자로 비동기 처리 결과를 전달합니다.
	- 이 처리 결과는 `Promise` 객체의 후속 처리 메서드로 전달됩니다.
3. 만약 비동기 처리에 실패하면 `reject` 메서드를 호출합니다. 이 때, `reject` 메서드의 인자로 에러 메시지를 전달합니다.
	- 이 에러 메시지는 `Promise`객체의 후속 처리 메서드로 전달됩니다.

<br>

---
## 1.3. Promise의 기본 형태
예제를 통해 더 자세히 알아보도록 하겠습니다.
```js
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('result');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure reason');
  }
});
```
문법적으로 보충 설명을 하겠습니다.

1. `Promise`객체의 변수는 `promise`이며 `const`로 선언했기 때문에 재할당이 되지 않습니다.
	- `Promise`를 관리할 때는 하나의 변수로 끝까지 관리하는 것이 가독성과 유지보수면에서 좋습니다.

2. `new Promise()`로 `Promise`객체를 새로 만들었습니다.

3. `Promise`의 생성자는 **특별한 함수 하나를 인자로 받습니다.** 여기서 인자로 들어가는 함수의 형태는 **화살표 함수**입니다.
	- 여기서의 특별한 함수 하나는 공식 문서에서 **executor**라는 이름으로 부릅니다.
    
### 1.3.1. executor
1. `executor`는 첫 번째 인수로 `resolve`, 두 번째 인수로 `reject`를 받습니다.
	- 변수명은 달라도 되지만 대체로 `resolve`와 `reject`로 정합니다.
    
2. `resolve`
	- 해당 함수를 호출하게 된다면 **이 비동기 처리 작업이 성공하였습니다.** 라는 의미입니다.
    
3. `reject`
	- 해당 함수를 호출하게 된다면 **이 비동기 처리 작업이 실패하였습니다.** 라는 의미입니다.
    
<br>

---
## 1.4. then & catch
`Promise`는 `new Promise`를 하는 순간 해당 객체에 할당된 비동기 작업이 **바로 시작됩니다.** 기다리지 않고 바로 호출해버린다는 겁니다.

비동기 작업의 특징은 **작업이 언제 끝날지 모른다**는 것입니다.
그럼 그 이후에 작업의 성공/실패 여부에 따라 그 다음 동작을 우리가 설정해주어야 하는데, 그것이 바로 **then**과 **catch**메서드입니다.

> **then** : 해당 Promise가 성공했을 때의 동작을 지정합니다.
> **catch** : 해당 Promise가 실패했을 때의 동작을 지정합니다.

- 위의 함수들은 **체인 형태**로 활용할 수 있습니다.
(연속적으로 호출할 수 있습니다.)

<br>

---
## 1.5. Promise 예제
```js
const promise1 = new Promise((resolve, reject) => {
  resolve();
});

promise1
  .then(() => {
    console.log("then!");
  })
  .catch(() => {
    console.log("catch!");
  });
```
```js
// 결과
then!
```
사실 기다리는 작업이 하나도 없다면 비동기를 쓸 이유는 없습니다.

그저 `resolve`가 실행되면 `then`에 있는 동작이 실행된다 라는 것만 알 수 있는 동작 방식만을 설명하기 위한 예제이죠.

<br>

```js
function startAsync(age) {
  return new Promise((resolve, reject) => {
    if (age > 20) resolve();
    else reject();
  });
}

const promise1 = startAsync(25);
promise1
  .then(() => {
    console.log("1 then!");
  })
  .catch(() => {
    console.log("1 catch!");
  });

const promise2 = startAsync(15);
promise2
  .then(() => {
    console.log("2 then!");
  })
  .catch(() => {
    console.log("2 catch!");
  });
```
```js
// 결과
1 then!
2 catch!
```

### 1.5.1. Consumer(then, catch) 간결하게 쓰기
```js
    // 정직한 코드
    getHen()
      .then((hen) => {
          return getEgg(hen);
       })
      .then((egg) => {
          return cook(egg);
       })
      .then((meal) => {
          console.log(meal);
       }) 

    // 간결한 코드
      getHen()
        .then(getEgg) // then이 받아오는 value를 바로 getEgg 함수에 전달
        .then(cook)
        .then(console.log);
```
인수가 한 개일 때는 함수 이름만 쓰면 암묵적으로 해당 함수가 매개변수로 전달되어 간결하게 작성할 수 있습니다.

<br>

***