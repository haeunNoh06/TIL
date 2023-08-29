# 파일 여러 개를 내 컴퓨터의 특정 폴더에 업로드 해보기

## 0. 개요
이번에는 php로 파일 여러개를 업로드해보겠습니다.

폴더 구성은 다음과 같습니다.
- `UPLOAD > uploads, input.html, upload.php`

<br>

## 1. html로 파일 여러개 첨부하기
먼저 파일을 첨부할 `html`코드입니다.
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="upload.php" method="post" enctype="multipart/form-data">
        첨부파일<input type="file" name="userfile[]" id="userfile" multiple><br>
        <button type="submit">업로드 완료</button>
    </form>
</body>
</html>
```
다중파일 업로드를 하기 위해서는 첨부 파일을 배열로 보내야합니다.
따라서 `name`부분에 배열 표시인 `[]`를 붙여주고, `multiple`태그를 붙여줍니다.

<br>

## 2. php로 특정 폴더에 다중 파일 업로드하기
특정 폴더에 내가 원하는 파일을 업로드하기 위해서는 아래와 같은 코드를 작성해야 합니다.

```php
<?php
// 업로드한 파일 php에서 보여주기
$upload_dir = "./uploads/";

// 업로드 폴더(directory)이 없으면
if ( !is_dir($upload_dir) ) {
    mkdir($upload_dir);
}

if ( isset($_FILES['userfile']['name'])) {
    $cnt = count($_FILES['userfile']['name']);

    for ( $i = 0; $i < $cnt; $i++) {
        if ( isset($_FILES['userfile']['name'][$i])
        && $_FILES['userfile']['size'][$i] > 0 ) {
            $filename = $_FILES['userfile']['name'][$i];
            $target = $upload_dir.$filename;
            $file = $_FILES['userfile']['tmp_name'][$i];
            if ( !move_uploaded_file($file, $target) ) {
                echo "실패";
                break;
            } else {
                echo "성공<br>";
            }
        }
    }
}
?>
```

```php
$upload_dir = "./uploads/";
```
`$upload_dir`에 내가 파일을 업로드할 폴더를 명시합니다.

<br>

```php
if ( !is_dir($upload_dir) ) {
    mkdir($upload_dir);
}
```
**is_dir()** 을 이용하여 있는 폴더가 또 생성되지 않도록 `if`문을 구성하였으며, 만약 폴더가 없을 경우에는 **mkdir()** 로 원하는 위치에 업로드 폴더를 만들어줍니다.

<br>

```php
if ( isset($_FILES['userfile']['name'])) {
```
만약 업로드한 파일이 존재한다면

```php
    $cnt = count($_FILES['userfile']['name']);
```
`count`를 통해 받아온 파일들의 갯수를 파악합니다.

**$_FILES** 는 `html`파일에 있는 `input`의 `name`에서 첨부한 파일들의 `'name`을 가져옵니다.
만약 첨부파일이 여러개라면 $_FILES는 해당 이름을 **배열**로 가져오게 됩니다.

`cnt`를 구하는 이유는 `for문`을 통해 파일들을 하나하나 만들어줄 것이기 때문입니다.

<br>

```php
    for ( $i = 0; $i < $cnt; $i++) {
        if ( isset($_FILES['userfile']['name'][$i])
        && $_FILES['userfile']['size'][$i] > 0 ) {
```
`for문`을 통해 모든 파일을 돕니다.

그리고 `if문`으로 간단한 유효성검사를 실행합니다.
만약 파일이 존재하지도, 파일 안에 담겨있는 내용이 있지도 않다면 곧장 프로그램은 종료되게 됩니다.

<br>

```php
            $filename = $_FILES['userfile']['name'][$i];
            $target = $upload_dir.$filename;
            $file = $_FILES['userfile']['tmp_name'][$i];
```
정상적으로 파일이 첨부되었다면

1. $_FILES로 파일의 이름을 가져오고
2. 내가 저장할 폴더와 파일명을 정하고
3. $_FILES를 사용하여 파일의 타입을 명시합니다.

<br>

```php
          if ( !move_uploaded_file($file, $target) ) {
                echo "실패";
                break;
            } else {
                echo "성공<br>";
            }
```
이제 내가 원하는 폴더에 첨부파일을 업로드합니다.

**move_uploaded_file()** 의 반환값은 `true or false`이므로 만약 파일이 제대로 업로드되지 않았다면 **실패**를, 제대로 업로드가 되었다면 **성공**을 출력하게 됩니다.

굳이 없어도 되지만 정상적으로 실행되었는지를 체크하기에 아주 좋은 수단입니다.

<br>

## 3. 배운점
`php`로 파일을 하나만 첨부하는 것은 알았는데 다중 파일 업로드 방식은 몰랐습니다.

만약 파일을 여러 개 첨부하게 되면 `$_FILES`로 가져왔을 때 배열로 갖고오게 된다는 것을 알게되었고,

자잘한 함수들인 `count()`, `isset()`, `move_uploaded_file()`에 대해 알 수 있었습니다.

이 코드를 활용하면 웹페이지를 만들었을 때 사용자가 첨부한 파일이 내 컴퓨터에 고스란히 들어올 수 있겠다는 생각이 들어 더 활용해보고 싶어졌습니다.

<br>