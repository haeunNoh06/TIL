# enter키를 누르면 자동 submit되는 에러

## ✏️발생배경
IT쇼를 준비하며 프로젝트를 진행하는 도중 이러한 문제가 발생하게 되었습니다.

- input type="text"인 상황에서 엔터를 누르면 `db`에 값이 저장되고 `php`페이지로 넘어가게 되는 오류

<br>

## ✏️발생원인
현재 화면에서 `input`으로 받은 값을 `php`로 `mysql`에 저장해야했기에
필요한 부분을 `form`태그로 걸었던 것이 문제가 되는 것 같았습니다.

그래서 검색을 해보니
http://www.w3.org/MarkUp/html-spec/html-spec_8.html#SEC8.2

When there is only one single-line text input field in a form, the user agent should accept Enter in that field as a request to submit the form.

라며 HTML 2.0 표준 스펙에 명시되어 있었습니다.

위의 문장을 번역해보면 **form 안에 text input field가 단 한 개만 있을 경우, 사용자가 Enter를 누르면 해당 폼을 제출(submit)한다.**라는 내용입니다.

<br>

하지만 값을 저장하기 위해서는 `form`태그 사용이 불가피했던 상황.
분명 이러한 현상이 저만 일어나는 것이 아니라는 것이라 짐작했고,
자동 `submit`을 막을 수 있는 무언가가 있을 것이라 생각하여 다시 "자동 submit을 막는 방법"을 검색한 결과 다음과 같은 해결방안을 찾을 수 있었습니다.

> form 태그의 속성값으로 막아주기

<br>

## ✏️문제해결
`form`태그에 `onsubmit="return false`를 해주어 자동 submit을 방지해주었습니다.

아래는 해결한 코드입니다.
```html
<form method="post" action="server.php" onsubmit="return false" name="user_name" id="user_name">
</form>
```

<br>

## 💡배운점
`type`이 `text`인 `input`이 `form`태그 안에 단 한 개 뿐일 때, 엔터를 치면 자동 `submit`이 된다는 사실을 알게 되었고, 이를 막기 위해서는 `onsubmit="return false"`를 해주어야 한다는 사실을 알게 되었습니다.

처음 이 에러를 맞닥뜨렸을 때는 왜 갑자기 `php`파일로 넘어가는지 이해가 되지 않았는데 `HTML 2.0` 표준 스펙에 명시되어 있는 것을 보고 이 문제를 해결할 수 있었습니다.

이제는 어떠한 에러가 발생하였을 때, 각 언어의 문서를 참고하면 해답이 있을 것이라는 걸 알게 되었네요!

<br>

***