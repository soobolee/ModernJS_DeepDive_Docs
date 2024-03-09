
# 스코프란

<br />

JS에선 `var`, `let` or `const`로 선언한 변수의 스코프는 다르게 동작한다.<br />
이는 스코프 즉, 변수, 함수의 `유효범위`와 깊은 관련이 있다.<br />

<br />

> 스코프는 식별자가 유효한 범위로 즉, `엔진이 식별자를 검색할 때의 범위`를 의미한다.

 <br />

```JavaScript
var var1 = 1; // 코드의 가장 바깥 영역에서 선언한 변수

if (true) {
  var var2 = 2; // 코드 블록 내에서 선언한 변수
  if (true) {
    var var3 = 3; // 중첩된 코드 블록 내에서 선언한 변수
  }
}

function foo() {
  var var4 = 4; // 함수 내에서 선언한 변수

  function bar() {
    var var5 = 5; // 중첩된 함수 내에서 선언한 변수
  }
}

console.log(var1); // 1
console.log(var2); // 2
console.log(var3); // 3
console.log(var4); // ReferenceError: var4 is not defined
console.log(var5); // ReferenceError: var4 is not defined
```

<br />

- 스코프가 있는 이유<br>
식별자는 어디에서든 유일해야 한다. 따라서, 식별자인 변수명은 중복될 수 없다.<br />
스코프를 통해 변수의 충돌을 방지한다.<br />

<br />

> 다른 스코프 내에서는 변수명의 `중복이 허용`된다. [var는 예외]

<br />

## 식별자의 결정

<br />

자바스크립트 엔진이 이름이 같은 두 식별자 중에 어떤 식별자를 참조해야 할 것인지 결정하는 것<br />

<br />

- 엔진은 코드를 실행 시 코드의 문맥을 고려<br />
- `문맥` = `코드 실행 위치`, `주변코드`를 고려하며 동일한 코드로 다양한 결과를 만들어낸다.<br />

<br />

```JavaScript
var x = "global";

function foo() {
  var x = "local";
  console.log(x); // local
}

foo();
console.log(x); // global
```

<br />

스코프를 통해 같은 이름의 변수를 사용가능하도록 만들지만 스코프에 따라 출력되는 값 또한 다르다.

<br />

## 스코프의 종류

<br />

- 전역<br />
  `코드의 가장 바깥`, `전역스코프`, `전역변수` = 어디서든 참조 가능<br />
<br />

- 지역<br />
  `함수 내부`, `지역스코프`, `지역변수` = 지역스코프 내와 스코프에 속한 하위 스코프 참조 가능<br />

<br />
  
## 스코프 체인

```JavaScript
var x = 10;

function outer() {
  var y = 20;

  function inner() {
    var z = 30;

    console.log(x); // 10
    console.log(y); // 20
    console.log(z); // 30  
  }
}
```

<br />

다음과 같은 예제가 있을 때 스코프 체인이 생성된다.<br />

<br />

> 스코프체인 = 스코프는 함수에 의해 중첩되어 `계층적 구조`를 가진다.
>
> 스코프체인을 상위부터 하위로 따라가면
>
> `전역스코프 -> outer스코프 -> inner스코프 가 된다.`

<br />

변수를 참조할 때 스코프체인은 엔진을 통해 변수를 참조하는 스코프에서 `상위 스코프로 움직이며` 값을 참조해오기 때문에<br />
inner 스코프에서 전역 스코프 또는 outer 스코프를 참조해올 수 있었던 것이다.<br />

<br />

위 개념은 함수에서도 마찬가지이다.
```JavaScript
// 전역 함수
function foo() {
  console.log("global function foo");
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log("local function foo");
  }

  foo(); // local function foo -> bar스코프에 존재하는 foo()를 호출
}

bar();
```

<br />

## 함수 레벨 스코프

<br />

- 블록 레벨 스코프<br />
  let과 const가 따르는 스코프로 함수 몸체 뿐만 아니라 `if`, `for`, `while`등이 지역 스코를 생성한다.<br />

<br />
  
- 함수 레벨 스코프<br />
  var가 따르는 스코프로 함수 몸체 내부에서만 스크프를 가진다.<br />

<br />

```JavaScript
var x = 1;

if (true) {
 // var 는 함수레벨 스코프를 가진다 따라서 블록레벨스코프 안에서는 var x = 1은 10으로 재할당된다.
  var x = 10;
}

console.log(x); // 10
```

<br />

## 렉시컬 스코프

<br />

JS가 따르는 스코프 결정방식으로 반대개념으로는 동적 스코프가 있다.<br />
<br />

- `동적 스코프` <br />
  함수가 호출되는 `시점` 동적으로 상위스코프를 결정<br />

<br />

- `정적 스코프`[렉시컬 스코프]<br />
  함수를 어디서 `정의`했는지에 따라 상위스코프를 결정한다.<br />
  JS 뿐만 아니라 대부분의 프로그래밍 언어가 따르는 스코프결정 방식이다.<br />

<br />

```JavaScript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo();
bar();
```

<br />

JS는 렉시컬 스코프 즉, `정적 스코프`를 따진다.<br />
따라서, 실행 시점에 함수 위치보다는 정의위치를 따르기 때문에<br />
해당 예제는 모두 `x = 1 즉, 1`을 `호출`한다.<br />

> 민약 동적 스코프를 따졌다면, 순서대로 `10`, `1`을 출력했을 것이다.
  

