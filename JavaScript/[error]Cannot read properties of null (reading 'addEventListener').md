# Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')

## ✏️발생배경
IT쇼를 준비하며 프로젝트를 진행하는 도중 이러한 문제가 발생하게 되었습니다.

- 체크되지 않았을 경우 빨간색으로 글씨 변경
- 하지만 label을 눌렀을 때 체크박스에 체크가 되지 않음

<br>

## ✏️발생원인
문제가 발생했던 코드는 다음과 같습니다.
```js
// 다음버튼이 눌렸을 때 체크박스가 체크되지 않은 경우
document.addEventListener('DOMContentLoaded', function() {
    // label 체크시 체크박스 checked
    
    
    const nextBtn = document.getElementById('next');
    nextBtn.addEventListener('click', function() {
        if ( !checkbox.checked ) {
            label.style.color = 'red';
        } else {
            label.style.color = 'black';
        }
    });
});

const checkbox = document.getElementById('myCheck');
const label = document.querySelector('label');

label.addEventListener('click', function() {
    checkbox.checked = !checkbox.checked;
});
```

원인을 알아보고자 `if ( !checkbox.checked )`에 `"성공"`을 출력해보았더니 
> Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')
라는 에러가 발견이 되었습니다.

찾아보니 **addEventListener함수를 호출할 때 null 객체의 속성을 읽으려했을 때 발생**하는 에러였습니다.

<br>

분명히 버튼도 잘 받아왔는데 어디서 `null`이 생긴건지 의아해다던 순간에 `DOMContentLoaded`의 특징 한 가지가 생각이 나게 되었습니다.

`DOMContentLoaded`는 Html문서가 로드될 때 실행이 됩니다.
따라서 `DOMContentLoaded`가 실행이 된 순간에는 `label`값을 받아오기 전이기 때문에 `null`값이 발생하게 되었던 것입니다!

<br>

## ✏️문제해결
따라서 `DOMContentLoaded`가 실행될 때 `label`값을 초기화시켜주어 `null`값이 발생하지 않도록 아래와 같이 고쳐주었습니다.
```js
document.addEventListener('DOMContentLoaded', function() {
    // label 체크시 체크박스 checked
    const checkbox = document.getElementById('myCheck');
    const label = document.querySelector('label');
    
    const nextBtn = document.getElementById('next');
    nextBtn.addEventListener('click', function() {
        if ( !checkbox.checked ) {
            label.style.color = 'red';
        } else {
            label.style.color = 'white';
        }
    });
});

label.addEventListener('click', function() {
    checkbox.checked = !checkbox.checked;
});
```

<br>

## 💡배운점
`DOMContentLoaded`은 `html`문서가 실행될 때 실행되기 때문에 초기화에 매우 용이하다는 사실을 깨닫게 되었습니다.
앞으로 `js`에서 어떠한 변수의 초기화가 필요하다면 `DOMContentLoaded`를 이용해서 초기화를 일괄적으로 할 수 있겠군요!