

# Ajax

자바스크립트를 사용하여 브라우저가 서버에게 `비동식 방식`으로 데이터를 요청하고,<br>
서버가 응답한 데이터를 수신하여 웹페이지를 `동적으로 갱신`하는 프로그래망 방식을 말한다.<br><br>

- Ajax는 브라우저에서 제공하는 `WebApi`인 `XMLHttpRequest` 객체를 기반으로 동작<br><br>

> 이전의 웹페이지는 html태그로 시작해서 html로 끝나는 완전한 html을 전송받아 웹페이지 전체를 처음부터 다시 렌더링 하는 방식
> - 단점으로는 화면이 깜박이는 현상과 불필요한 부분도 다시 렌더링 해야하는 점
> 
> 그러나 Ajax가 등장하며 부드러운 화면 전환 효과를 보여주었다.
> - 비동기로 원하는 부분의 html만 변경하여 효율적으로 변함

<br>

## JSON

JSON은 클라이언트와 서버 간 `HTTP` 통신을 위한 텍스트 데이터 포맷이다. <br>
JS에 종속되지 않는 언어 독립형 데이터 포맷으로 대부분의 프로프래밍 언어에서 사용 가능하다.<br><br>

1. JSON 표기방식<br><br>

JSON은 객체 리터럴과 유사하게 키와 값으로 구성된 `순수한 텍스트`이다.<br>

```JavaScript
{
"name"  : "Lee",
"age"   : 20,
"alive” : true,
"hobby" : ["traveling", "tennis"]
}
```

<br>

2. JSON.stringify<br><br>

JSON.stringify 메서드는 `객체를` JSON 포맷의 `문자열로` 변환한다. <br>
클라이언트에서 서버로 객체를 전송하려면 문자화해야 하는데 이를 직렬화 라고 한다.<br>

```JavaScript
const obj = {
name: 'Lee',
age: 20,
alive: true,
hobby: ['traveling'f ’tennis']
}；
// 객처» JSON 포맷의 문자열로 변환한다
.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age"：20,"alive":true,"hobby["traveling","tennis"]}
```

<br>

3. JSON.parse<br><br>

JSON.parse 메서드는 JSON 포맷의 `문자열을 객체로` 변환한다.<br>
서버로부터 전달받은 문자열 JSON 포맷을 JS에서 사용하려면 다시 객체화 해야하는데 사용<br>
이를 역질렬화 라고 한다.<br>

```JavaScript
const todos = [
  { id: 1, content: ’HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'JavaScript’, completed: false }
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환한다. 배열의 요소까지 객체로 변환된다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);

/*
object [
  { id: 1, content: ’HTML', completed: false },
  { id: 2, content: ’CSS’, completed: true },
  { id: 3, content: 'JavaScript', completed: false }
]
*/
```

<br>

## XMLHttpRequest

JS를 이용해 HTTP 요청을 전송하려면 `XMLHttpRequest` 객체를 사용한다.<br>
WebApi인 `XMLHttpRequest` 객체는 HTTP 요청 전송과 응답수신을 위해 지원된다.<br><br>

1. XMLHttpRequest 객체 생성<br><br>

```JavaScript
const xhr = new XMLHttpRequest();
```

<br>

2. XMLHttpRequest 객체의 프로퍼티와 메서드<br><br>
   
<a href="https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest">XMLHttpRequest 객체 자세히 보러가기</a>

- 프로토타입 프로퍼티<br>
  readyState<br>
  status<br>
  statusText<br>
  responseType<br>
  response<br>
  responseText<br>

- 이벤트 헨들러 프로퍼티<br>
  onreadystatechange<br>
  onloadstart<br>
  onprogress<br>
  onabort<br>
  onerror<br>
  onload<br>
  ontimeout<br>
  onloadend<br>

- 정적 프로퍼티<br>
  UNSENT<br>
  OPENED<br>
  HEADERS_RECEIVED<br>
  LOADING<br>
  DONE<br>

- 메서드<br>
  open<br>
  send<br>
  abort<br>
  setRequestHeader<br>
  getResponseHeader<br><br>


## XMLHttpRequest 예제

```JavaScript
const xhr = new XMLHttpRequest();

xhr.open('GET', 'https://jsonplaceholder.typicode.eom/todos/');

xhr.send();

xhr.onreadystatechange = () => {
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.log("error!!!! 발생발생");
  }

}
```




