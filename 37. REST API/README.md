

# REST API

REST(REpresentational State Transfer)는 HTTP/1.0과 1.1의 스펙 작성에 참여했고, <br>
아파치HTTP 서버 프로젝트의 공동 설립자인 로이 필딩의 논문에서 처음 소개되었다.<br><br>

소개 당시 웹이 HTTP를 제대로 사용하지 못하고 있는 상황을 보고 HTTP의 장점을 최대한<br>
활용할 수 있는 REST를 소개했고, REST기본 원칙을 잘 지킨 서비스 디자인을<br>
`Restful` 하다고 표현한다.<br><br>

즉, REST 는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍쳐이다.<br><br>

# 구성

REST API는 `자원`, `행위`, `표현`의 3가지 요소로 구성된다.<br>
위 3가지 요소로만 HTTP요청의 내용을 이해할 수 있다.<br><br>

|구성요소|내용|표현방법|
|------|---|---|
|자원|자원|URI|
|행위|자원에 대한 행위|HTTP 요청 메서드|
|표현|자원에 대한 행위의 구체적 내용|페이로드|

<br>

# REST API 설계 원칙

가장 중요한 원칙은 두 가지로<br>
- URI는 리소스를 표현하는데 `집중`하고<br>
- 행위에 대한 정의는 HTTP 요청 `메서드를 통해` 한다.<br><br>

1. URI는 리소스를 표현해야 한다.<br>

리소스를 식별할 수 있는 동사보다 명사를 사용한다.<br><br>
```JavaScript
// bad
GET /getTodos/1

// good
GET /todos/1
```

<br>

2. 행위에 대한 정의는 HTTP 요청 메서드를 통해 한다.<br><br>

|HTTP 요청 메서드|종류|목적|페이로드|
|------|---|---|---|
|GET|index/retrieve|모든/특정 리소스 취득|X|
|POST|create|리소스 생성|O|
|PUT|replace|리소스 전원 교체|O|
|PATCH|modify|리소스 일부 교체|O|
|DELETE|delete|모든/특정 리소스 삭제|X|

```JavaScript
// bad
GET /getTodos/delete/1

// good
DELETE /todos/1
```


