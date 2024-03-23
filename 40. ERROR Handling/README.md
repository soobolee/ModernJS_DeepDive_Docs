
# 에러 처리

에러가 발생하지 않는 코드를 작성하는 것은 `불가능`에 가깝다.<br><br>

때문에 다양한 방법으로 `예외` / `에러`를 처리해야 한다.<br><br>

1. 예외적인 상황이 발생하면 반환하는 값 `null`, `NaN`, `undefined`를 통해 조건문을 걸어 확인 후 처리하는 방법<br>
2. 에러처리 코드를 미리 등록해두고 에러가 발생하면 해당 코드가 실행되도록 하는 방법<br><br>

발생한 에러는 에러 관련 처리를 해주지 않으면 에러를 만나는 즉시 프로그램이 `종료`된다.<br><br>

```JavaScript
console.log("start");

foo();

console.log("end");

// 'start'
// ReferenceError : foo is not defined
```

<br>

하지만 에러 처리를 적절하게 해주면 프로그램이 강제 종료되는 것을 `방지`할 수 있다.<br><br>

```JavaScript
console.log("start");

try {
  foo();
} catch (err) {
  console.log("에러 있다!");
}

console.log("end");

// 'start'
// '에러 있다!'
// 'end'
```

<br>

위 예제처럼 `try...catch`문을 사용하여 에러를 처리할 수 있다.<br><br>

JS는 컴파일 언어가 아니기 때문에 사전에 에러의 유무 발생 위험을 알 수 없다.<br>
경고해주지 않음<br>
- 때문에 에러나 예외적인 상황이 발생한다는 것을 전제로 `에러 대응 코드`를 작성해야 한다.<br>

<br>

## try...catch...finally 문

```JavaScript
try {
  // 실행할 코드
} catch (err) {
  // try 블록에서 에러가 발생 시 이 코드 실행
  // 인수로는 try 블록에서 발생한 에러 객체 전달
} finally {
  // 에러 유무와 상관없이 반드시 한 번 실행
}
```

<br>

## ERROR 객체

Error 생성자 함수로 에러 객체를 `생성`한다.<br>

인수로는 에러에 대한 상황을 설명할 수 있도록 문자열을 받는다.<br>
```JavaScript
const error = new Error("error 발생 빨랑 고쳐서 배포하세요");
```

<br>

- Error 객체 프로퍼티<br>
  1. `message` : Error 생성자 함수 호출 시 인수로 전달한 메시지<br>
  2. `stack`   : 에러를 발생 시킨 콜 스택의 호출 정보를 나타내는 문자열<br><br>

- Error.prototype을 상속받은 Error 객체들<br><br>

  1. `Error`<br>
     - 일발전 에러 객체<br>
  2. `SyntaxError`<br>
     - 문밥에 맞지 않아서 나는 에러 객체<br>
  3. `ReferenceError`<br>
     - 참조할 수 없는 식별자 참조했을 때 반환되는 객체<br>
  4. `TypeError`<br>
     - 식별자의 데이터 타입이 틀릴 때 발생<br>
  5. `RangeError`<br>
      - 숫자값의 허용범위 벗어났을 때 나는 에러<br>
  6. `URIError`<br>
      - encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달 시 발생<br>
  7. `EvalError`<br>
      - eval함수에서 발생하는 에러 객체<br><br>


## throw 문

Error로 에러 객체를 만들었다고 에러 발생하는 것이 아니고<br>
해당 에러 객체를 던쟈야 에러가 발생되는데<br>
에러를 던질 때 `throw`를 사용한다.<br><br>

```JavaScript
try {
  // 사용자가 직접 정의한 에러를 catch로 던질 수 있다.
  if ("에러 발생시키고 싶어요") {
    throw new Error("Error 발생했어용~");
  }
} catch (err) {
  console.log("에러 발생 :", err); // 에러 발생 : Error 발생했어용~
}
```

<br>

## Error의 전파

```JavaScript
const foo = () => {
  throw new Error("foo 에서 발생한 에러"); // 4
};

const bar = () => {
  foo(); // 3️
};

const baz = () => {
  bar(); // 2️
};

try {
  baz(); // 1️
} catch (err) {
  console.error(err);
}

// Error: foo 에서 발생한 에러
```

위 예제처럼 `호출자 방향`으로 Error가 전파되는 것을 확인할 수 있다.<br>
즉, 콜 스택[실행 컨텍스트]의 `아래 방향`으로 전파된다.<br><br>

|콜 스택에 들어온 순서|코드|전파 방향|
|------|---|---|
|4|foo 실행 컨텍스트|에러 발생 첫번째|
|3|bar 실행 컨텍스트|에러 위임 받음1|
|2|baz 실행 컨텍스트|에러 위임 받음2|
|1|전역 실행 컨텍스트|에러 실행|

- throw는 에러를 상위 코드로 위임하는 것이기 때문에 상위 스코프 어디에서도 catch 하지 않으면 프로그램은 `여전히 강제 종료 된다.`
