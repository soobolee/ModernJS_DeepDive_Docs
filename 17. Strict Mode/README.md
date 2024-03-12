
# strict mode

<br>

JS는 `암묵적`이라는 말이 참 많이 나온다, 그 만큼 우리가 예측하기 힘든 `오류`를 많이 발생시킬 수 있다.<br><br>

다은 예제를 보자<br>
```JavaScript
function foo() {
  x = 10;
}
foo();

console.log(x);
```

<br>

foo 함수 내에에서 x를 찾아야 값을 할당가능하기 때문에 x가 어디에 선언되었는지 `스코프체인`을 통해 검색해 나간다.<br>

<br>

1. JS엔진은 먼저 `foo 함수 내`에서 x변수를 검색한다.<br>
2. 없으므로, `상위 스코프로` 검색범위를 넓혀, x변수의 선언을 검색한다.<br>
3. 전역스코프에도 없다는 것을 알게되었다.<br>
4. ReferneceError를 발생시키지 않고, `암묵적으로 전역 객체에 프로퍼티 x를 동적 생성`<br>
5. 암묵적으로 전역 객체를 생성하는 `암묵적 전역현상`이 발생<br>

<br>
위처럼 개발자의 의도와 상관없이, 발생한 암묵적 전역은 `오류를 발생` 시키는 원인이 된다.<br><br>

> 따라서, 반드시 `var`, `let`, `const` 키워드를 사용해 변수를 선언해야 한다.

<br>

하지만 인간은 실수의 동물이다. 언제나 실수를 할 수 있다 때문에 사전에 막는 것이 필수이다.<br>
때문에 ES6에서 `strict mode[엄격 모드]` 가 추가되었다.<br>

<br>

이 모드는 문법을 좀 더 엄격히 적용하여 `오류를 발생시킬 가능성이 높거나` `최적화에 방해`되는 코드에 대해 `명시적 에러`를 발생시킨다.<br><br>

## 적용

<br>

```JavaScript
'use strict';

function foo() {
// 'use strict';
  x = 10;
}
foo();
```

<br>

전역 또는 함수의 몸체에 위치시키면 적용은 해당 `use strict가 적힌 스코프를 범위로 문법을 강화`한다.<br>
<br>

***꼭 선두에 위치해야만 함, 코드 밑에서는 위에 코드 `적용X`***


## 전역에 적용하는 것은 피하자.

<br>

전역에 적용한 strict mode는 스크립트 단위로 적용된다.<br>
하지만, strict mode와 non-strict mode를 혼용하는 것은 오류를 발생시킬 수 있다.<br>
특히 외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode를 지원하는 경우가 있기 때문이다.<br>
전역에 필요한 경우 `즉시 실행함수`를 이용하여 모드를 적용하자.<br><br>

```JavaScript
(function() {
  'use strict';

}())
```

## 함수 단위로 적용하는 것도 피하자.

<br>

모든 함수에 strict mode를 적용하는 것도 번거로우며, 어떤 함수는 사용하고 어떤 함수는 사용하지 않는 경우가 발생할 수 있기 때문에 바람직하지 않다.<br>
<br>
꼭 필요한 경우에만 `즉시 실행 함수`로 감싼 스크립트 단위로 적용하는 것이 바람직하다.<br><br>

## strict mode가 발생시키는 에러

<br>

1. `암묵적 전역`<br>
   기존 암묵적 전역으로 넘어가던 에러도 이제는 명시적으로 표시된다.<br><br>
   
2. `변수, 함수, 매개변수의 삭제`<br>
   delete연산자로 변수, 함수, 매개변수를 삭제하면 SystaxError가 발생한다.<br><br>

3. `매개변수 이름의 중복`<br>
   중복된 매개변수 이름을 사용하면 에러가 발생<br><br>

4. `with 문의 사용`<br>
   전달된 객체를 스코프 체인에 추가해주는 with문은 성능과 가독성이 나빠지는 문제가 있어서 에러를 발생시킴<br><br>

## strict mode 적용에 대한 변화

<br>

1. `일반 함수의 this`<br>

```JavaScript
// strict mode에서 함수를 일반함수로 호출하면 this에 undefined라 바인딩 된다.
// 일반 함수 내부에서는 this가 바인딩되기 때문이다.

(function () {
  ("use strict");

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
})();
```

<br>

2. `arguments 객체`<br>

```JavaScript
// strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

(function (a) {
  ("use strict");

  a = 2;

  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```

