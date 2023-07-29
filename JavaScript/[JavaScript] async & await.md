# 1. async & await
> **async & await** : 비동기식 코드를 동기식으로 표현하여 간단하게 나타내는 것

`async function`과 `await`키워드는 `Promise`의 결과값을 `then`과 `catch`를 통해 다루지 않고,
변수에 담아 동기적 코드처럼 작성해줄 수 있다는 점에서 편리함을 제공합니다.

또한 `async`와 `await`는 `Promise`객체를 반환하며 `=> then`을 사용할 수 있습니다.

<br>

## 1.1. async과 await의 형태
```js
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```

<br>

## 1.2. async 란?
> **async** : 비동기 작업을 만드는 손쉬운 방법

- `async`함수는 **함수를 선언할 때** 붙여줄 수 있습니다.

- `async`가 붙은 함수는 항상 **Promise를 반환**합니다.
  - promise가 아니라면 promise로 감싸 반환합니다.

<br>

### 1.2.1. async는 왜 사용하나요?
> 기존의 비동기 처리 방식인 콜백 함수와 프로미스의 단점을 보완할 수 있습니다!

특히나 복잡했던 `Promise`를 조금 더 편하게 사용할 수 있습니다.

<br>

## 1.3. await 란?
> **await** : promise가 처리될 때까지 함수 실행을 기다리게 만든다.

`await`는 `async`함수 안에서만 동작합니다.
`await`라는 말 그대로 `promise`가 처리될 때까지 기다린 후 결과가 반환됩니다.

<br>

## 1.4. async &  await 예제
```js
		async function f() {

            let promise = new Promise((resolve, reject) => {
                setTimeout(() => resolve("완료!"), 1000)
            });

            let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)

            console.log(result); // "완료!"
        }

        f();
```
```js
// 결과
완료!
```

1. `f`함수를 호출한 뒤 함수 본문이 실행되는 도중 `await promise`에서 실행이 잠시 중단됩니다.

2. 1초 뒤 `result`의 값이 할당되어지고 `완료!`가 출력됩니다.

<br>

```js
        function delay(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }

        //2초동안 기다리게 하고 사과를 리턴하는 메서드
        async function getApple() {
            await delay(2000)
            return '🍎'
        }

        //1초동안 기다라게 하고 사과를 리턴하는 메서드
        async function getBanana() {
            await delay(1000)
            return '🍌'
        }

        getApple().then(console.log)  // 결과 : 🍎
        getBanana().then(console.log) // 결과 : 🍌
```
```js
// 결과
🍎
🍌
```

<br>

## 1.5. async 와 await를 사용하는 이유
위에서 사과와 바나나를 출력하였을 때 생각보다 단순한 코드 진행을 볼 수 있었습니다.

> 이렇듯 async 와 await를 사용하면 **예측이 불가능했던 비동기 작동 방식이 동기적으로 실행되는 코드처럼 예측이 가능**해질 수 있습니다.

<br>

## 1.6. async await 에러 제어
`async`와 `await`를 사용하면 `await`가 대기하는 것을 처리해주기 때문에 `.then`이 거의 필요하지 않습니다.

또한 `promise`에서 에러를 제어해주던 `.catch`대신 **try-catch**를 사용할 수 있다는 장점도 있습니다.

<br>

## 1.7. 콜백 함수와의 차이점은?
`callback` `promise` `async/await`는 `js`에서 비동기를 처리할 수 있게 하는 방법들이지만 각각 약간씩의 차이점이 존재합니다.

### 1.7.1. callback
- 함수를 매개변수로서 다른 함수에 전달하고 감싼 함수의 내부에서 해당 함수를 호출.
- 콜백을 넘겨받는 코드는 이 콜백을 필요에 따라 즉시 실행할 수도 있고, 아니면 나중에 실행할 수도 있음
- 코드의 가독성이 떨어지며 에러 처리가 어려워짐

### 1.7.2. promise
- 비동기 연산이 종료된 이후에 결과값과 실패 사유를 처리하기 위한 처리기를 연결
- new Promise를 하는 순간 해당 객체에 할당된 비동기 작업이 바로 시작됨
- `catch`로 에러 처리 가능

### 1.7.3. async / await
- `await`로 `promise`가 다 처리될 때까지 기다려, 비동기식 코드를 동기식으로 표현하여 간단하게 나타냄
- `try, catch`로 에러 처리 가능

<br>

***