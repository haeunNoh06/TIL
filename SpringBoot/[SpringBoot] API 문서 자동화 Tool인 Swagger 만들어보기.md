# API 문서 자동화 Tool인 Swagger 만들어보기

## 1. 의존성 추가
build.gradle에 아래의 코드를 추가합니다.
이는 springdoc 라이브러리를 사용하여 swagger를 만들어보겠다는 뜻입니다.
```java
dependencies {
	...
	implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.2.0'
}
```

<br>

## 2. Swagger 사용을 위한 Config 만들기
`config` 패키지를 하나 만들고 그 안에 `WebConfig`라는 자바 파일을 하나 만들겠습니다.
![](https://velog.velcdn.com/images/1109_haeun/post/9044ad7a-794f-4748-b7f3-74603eabe134/image.png)

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOriginPatterns("*")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("Authorization", "Content-Type")
                .exposedHeaders("Custom-Header")
                .allowCredentials(true)
                .maxAge(3600);
    }
}
```
주의할 점은 `.allowedOrigins("*")`이 `.allowedOriginPatterns("*")`로 바뀌었다는 점입니다.

<br>

## 3. Swagger 접속하기
이제 준비가 완료되었습니다.
http://localhost:8080/swagger-ui/index.html#/
로 이동하면 내가 만든 api를 한 눈에 볼 수 있습니다.