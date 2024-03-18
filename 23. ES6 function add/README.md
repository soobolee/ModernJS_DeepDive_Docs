
# ES6 함수 추가 기능

## 함수의 구분

ES6 이전의 모든 함수는 일반함수로써도 new연산자를 사용해 생성자함수도 호출이 가능했다.<br>
즉 모든 함수는 `callable`이면서 `constructor`를 모두 가지고 있다.<br><br>

개발자가 사용용도를 명확히 하고, 일반함수 & 생성자함수를 모두 하는 역할의 함수를 만드는 경우도 없었기 때문에<br>
문제가 없어보이겠지만, <br>
둘 다 가능하다는 것은 성능면에서 문제가 생긴다.<br><br>

예를 들어 함수에 전달되는 보조함수 기능을 하는 콜백함수도 constructor를 가지므로 불필요한 프로토타입 객체를 생성한다.<br>
이러한 제약이 존재하지 않는 함수는 실수를 유발하기도 하며, 성능에도 좋지않다.<br><br>

- 때문에 문제를 해결하기 위해 ES6 에서는 함수를 사용목적에 맞게 세 가지 종류로 명확히 구분했다.<br>

  |ES6 함수 구분|constructor|prototype|super|arguments|
  |------|---|---|---|---|
  |일반 함수|O|O|X|O|
  |메서드|X|X|O|O|
  |화살표 함수|X|X|X|X|

  일반함수는 함수 선언문이나 함수 표현식으로 정의한 함수를 말하며 ES6 이전의 함수와 크게 차이가 없다.<br>
  메서드는 객체 안에서 `축약표현`으로 정의된 함수를 말한다.<br><br>

## 메서드

ES6 이후, 메서드는 메서드 축약표현으로 정의된 함수만을 의미한다.<br><br>

```JavaScript
const obj = {
  x: 1,
  // foo는 ES6 기준, 메서드다.
  foo() {
    return this.x;
  },
  // bar는 ES6 기준, 메서드가 아니다.
  bar: function () {
    return this.x;
  },
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```
 - ES6 사영에서 정의한 메서드는 인스턴스를 생성할 수 없는 `non-constructor`이다.<br>
     - 따라서 생성자 함수로 호출이 불가능하다.<br><br>

 - ES6 메서드는 non-contructor이므로 `인스턴스`를 생성할 수 없다.<br>
     - 즉, `불필요`한 prototype을 생성하지 않는다.<br><br>

 - ES6에서 메서드는 자신을 바인딩한 객체를 가리키는 `[[HomeObject]]`를 내부 슬릇으로 갖는다.<br>
 - ES6에서 메서드는 super 키워드를 사용할 수 있다.<br><br>

## 화살표 함수

ES6의 화살표 함수는 기존의 함수 정의를 보다 간략하고 간편하게 정의하는 방식이다.<br>

- 화살표 함수의 정의<br>

```JavaScript
const arrowFun = () => {};
```

- 매개변수 선언<br>
```JavaScript
// 매개변수가 여러개일 때 
const arrowFunc = (x, y) => {};

// 매개변수 하나일 때
const arrowF = x => {}

// 매개변수 없음
const arrowF2 = () => {};
```

- 함수 몸체 정의<br>
```JavaScript
const power = x => x + 2; // 암묵적으로 리턴

power(4); // 6

const power2 = x => let x = 2; // Error
//위는 이것과 같다 - const power2 = x => { return let x = 2};
```
  - 함수 몸체의 하나의 값으로 평가되는 표현식인 문이 하나일 경우 {}는 `생략`이 가능하다.<br>
  - 값으로 평가되는 표현식일 경우 `암묵`적으로 return 한다.<br>
  - 반환값이 따로 존재할 경우 return을 `명시` 해야한다.<br>
  - 화살표 함수도 `일급객체`이다.<br><br>


## 일반 함수와 화살표 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다.<br>
```JavaScript
const foo = () => {};
new foo(); // TypeError
```

2. 중복된 매개변수 이름을 선언할 수 없다.<br>
```JavaScript
function foo(a, a) {
  return a + a;
}
// SysntaxError
// use strict 모드와 마찬가지
```

3. 화살표 함수는 함수 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않는다.<br>
   - 화살표 함수에서 위 항목을 참조하면, `상위 스코프` 항목을 참조한다.<br><br>

## this

화살표 함수의 this는 일반 함수와 다르게 동작한다.<br><br>

그 이유는<br>
- 콜백함수 내부의 this문제를 해결하기 위한 것<br><br>

  - `콜백함수 내부의 this문제`<br>
    일반함수로 정의된 콜백 함수는 내부에서 this참조 시 `전역객체`가 바인딩되는 문제가 있다.<br>
    BUT 화살표 함수는 바로 위 상위스코프를 탐색해 나가면서 this를 참조한다.<br>
    이는 this가 함수가 정의된 위치에 의해 결정되는 `렉시컬 디스[Lexical this]`라고 한다.<br><br>

```JavaScript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    // this 는 상위스코프를 탐색하므로 Prefixer 객체를 참조한다.
    return arr.map(item => this.prefix + item);
  }
}

const prefixer = new Prefixer('hihi');
console.log(refixer.add(['soo', 'bo']));
// hihisoo, hihibo
```

## super, arguments

this와 마찬가지로 `상위스코프`의 super, arguments를 참조한다.<br><br>

arguments의 경우 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현 시 유용하지만<br>
화살표 함수에서는 arguements를 객체로 `사용할 수 없으`므로, <br>
자신에게 전달된 인수를 확인 불가하고 상위 스코프의 인수만 <br>
확인하므로 쓸 일이 많지 않다.<br><br>

대신에 `Rest 파라미터`를 주로 사용한다.<br><br>

## Rest 파라미터

함수의 매개변수를 받는 부분에서 매개변수 이름 앞에 `점 세게 [...]`을 붙여 사용한다.<br><br>

- Rest파라미터는 함수에 전달된 인수들의 목록을 `배열`로 전달 받는다.<br>
- 일반 매개변수와 Rest 파라미터는 `혼용`해서 사용 가능하다. [Rest파라미터의 위치는 `맨 뒤`]<br>
- Rest 파라미터는 `단 하나`만 선언<br>
- 매개변수의 `length 프로퍼티`에 영향을 주지 않는다.<br>

```JavaScript
function foo(...param) {
  console.log(rest); // [1, 2, 3]
}

foo(1, 2, 3);

function foo(x, y, ...rest) {} // O
function foo(...rest, ...rest2) // X

function bar(x, ...rest) {}
console.log(bar.length); // 1
```

## 매개변수의 기본값

JS에서는 매개변수의 개수와 인수의 개수를 `체크하지 않는다.`<br> <br>

따라서, 의도치 않은 아래의 상황이 발생할 수 있다.<br><br>
```JavaScript
function sum(x, y) {
  return x + y;
}

sum(4); // NaN
```

따라서 매개변수의 인수가 제대로 전달되지 않았을 경우에 대비해 `기본값`을 사용할 수 있다.<br><br>

```JavaScript
function sum(x, y = 3) {
  return x + y;
}

sum(4); // 7
```






























