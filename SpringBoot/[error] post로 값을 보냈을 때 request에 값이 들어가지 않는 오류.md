# post로 값을 보냈을 때 request에 값이 들어가지 않는 오류
## ✏️1. 발생배경
```html
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"
        integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        let paper_title;
        let receiver;

        function create() {
            console.log('onclick!');
            var paper_title = $("#paper-title").val();
            var receiver = $("#receiver").val();

            let param = {
                "papertitle": paper_title,
                "paperreceiver": receiver
            };
            console.log(param);

             // 2. ajax 함수 이용해서 api 호출, success 응답 호출 해보기
            $.ajax({
                type: 'post',           // 타입 (get, post, put 등등)
                url: 'http://localhost:8080/api/rolling-papers',           // 요청할 서버url
                async: true,            // 비동기화 여부 (default : true)
                dataType: 'json',       // 데이터 타입 (html, xml, json, text 등등)
                data: JSON.stringify(param),
                contentType: "application/json",
                success: function (data) { // 결과 성공 콜백함수
                    console.log(data);
                }
            });
            }
       
    </script>
```
ajax를 사용하여 springboot로 만든 api에 제목, 받을 대상의 값을 post로 넘겨 저장하려고 했는데

![](https://velog.velcdn.com/images/1109_haeun/post/32e825f2-aa48-45c0-8d61-1354c09c9309/image.png)제목과 받을 대상을 입력해도

![](https://velog.velcdn.com/images/1109_haeun/post/af7f916f-dad9-4eda-989f-61e6ef88f5e1/image.png)api에는 값이 들어가지 않는 오류가 발생했습니다.


<br>

## ✏️2. 발생원인
> 알고보니 `param`의 key값과 api의 변수명이 동일해야 해당 값이 제대로 들어가는 것이었습니다.

`param`의 제목 변수는 `papertitle`이었지만 api에서의 제목 변수는 `title`이었기 때문에 값이 제대로 넘어가지 않았던 겁니다.
`paperreceiver`와 `receiver`도 동일한 이유입니다.

<br>

## ✏️3. 문제해결
```js
			let param = {
                "title": paper_title,
                "receiver": receiver
            };
```
`param`의 key를 api와 동일하게 `title`, `receiver`로 변경해주고

![](https://velog.velcdn.com/images/1109_haeun/post/ca76502c-27ce-478c-b36f-c65e75c16272/image.png)제목과 받을 대상에 값을 입력해주면

![](https://velog.velcdn.com/images/1109_haeun/post/69168467-2623-49d2-92f9-8977bf0ed797/image.png)이렇게 값을 가져와 저장한 것을 확인할 수 있었습니다.

<br>

## 4. 💡배운점
사실 springboot로 api를 만들어서 활용해본 것이 이번이 처음이라고 할 수 있는데 에러를 해결하는 과정에서 api를 조금 다룰 수 있게 된 것 같아 개인적으로 성장했다고 느꼈습니다.

또한 api의 변수명과 데이터를 넘겨주는 키값이 동일해야 한다는 사실을 깨닫게 되었습니다. 