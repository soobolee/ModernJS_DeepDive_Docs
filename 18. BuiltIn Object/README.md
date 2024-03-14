
## 빌트인 객체

<br>

자바스크립트 객체는 다음과 같이 크게 3가지로 분류된다.<br><br>

1. 표준 빌트인 객체<br>
   ECMAScript 사양에 정의된 객체를 의미, 애플리케이션 전역에 공통기능을 제공, 전역 객체의 프로퍼티로 제공되므로 `별도의 선언 없이` 전역 변수처럼 언제나 참조가능<br><br>

Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, <br>Function, Promise, Reflect, Proxy, JSON, Error 등 40여개의 표준 빌트인 객체를 제공한다.
<br><br>
Math, Reflect, JSON을 제외한 모든 객체는 생성자 함수 객체이다,<br>
생성자 객체는 프로토타입 메서드와 정적 메서드를 제공하며 `아닌 객체는 정적 메서드만 제공한다.`<br>
<br>

> 정적 메서드 : 인스턴스 정의 없이 바로 사용 가능한 메서드

<br>

2. 호스트 객체<br>
   ECMAScript 사양에 정의되어 있지 않지만, 브라우저 또는 `Node.js` 환경에서 추가로 제공하는 객체를 의미<br>
   대표적으로 브라우저의 `DOM`, `BOM` 객체가 있다.<br><br>
   
4. 사용자 정의 객체<br>
   사용자가 `직접 정의`한 객체<br><br>

## 원시 값과 래퍼 객체

문자열이나 숫자, 불리언 등의 원시값 타입이 존재하는데도, 객체를 생성하는 Boolean, Number같은 `표쥰 빌트인 생성자 함수`가 있는 이유는??<br>
<br>
원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도, 아래에서는 마치 객체처럼 정적메서드를 사용하고 있다.<br>
```JavaScript
const str = 'hi';

console.log(str.length); // 2
console.log(str.toUpperCase()); // HI
```

이는 원시값인 문자열, 숫자, 불리언 값의 경우 이들에게 객체타입의 프로퍼티에 접근하는 것처럼 접근하면, <br>
JS엔진이 `암묵적으로 연관된 객체로 변환` 시켜주기 때문이다.<br>

> 즉, 원시값을 객체처럼 사용하면 `연관된 객체`를 생성 후 `프로퍼티에 접근`한 뒤에 다시 `원시값으`로 되돌린다.<br><br>

이러한 원시값에 객체처럼 접근하면 생성되는 임시 객체를 ***래퍼 객체***라고 한다.<br>

<br>

## 전역 객체

전역 객체는 JS가 실행되기 이전 엔진에 의해 어떤 객체보다도 먼저 생성되는 객체이다. 어떤 객체에도 속하지 않는다.<br>
<br>
- 브라우저에서는 window<br>
- node.js 에서는 global<br><br>

### 특징

- 전역객체는 개발자가 의도적으로 생성할 수 없다. 즉, 전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다.<br>
- 전역 객체의 프로퍼티를 참조 시 전역객체명을 `생략`할 수 있다.<br>
- 여러개의 스크립트 파일이 있다고 해도 `하나의 window를 공유`한다.<br><br>

var 키워드로 선언한 전역 변수와,<br>
그 어떤 키워드로도 선언하지 않은 함묵적 전역 변수 그리고 전역함수는<br>
전역 객체의 프로퍼티가 된다.<br>

```JavaScript
var foo = 1;
console.log(window.foo) // 1

bar = 2; // 암묵적 전역
console.log(window.bar) // 2

function baz() { return 3 }
console.log(baz()) // 3

// let, const 은 제외
let loo = 1;
console.log(loo) // undefined
```

## 빌트인 전역 프로퍼티

1. Infinity<br>
   무한대를 나타내는 숫자값을 찾는다.<br><br>

2. NaN<br>
   숫자가 아닌 것을 찾는다.<br><br>

3. undefined<br>
   undefined를 값으로 갖는다.<br><br>

## 빌트인 전역 함수

1. eval<br>
   JS코드를 나타내는 문자열을 받아 런타임에 평가 및 생성하여 실행한다. XSS, 최적화방해, 느림에 따라 사용자제<br><br>

2. isFinite, isNaN<br>
   유한수인지 무한수인지 검사한다, NaN인지 검사한다.<br><br>

3. parseFloat, parseInt<br>
   전달받은 문자열울 실수, 정수로 해석 후 반환한다.<br><br>

4. encodeURI, decodeURI<br>
   완전한 URI를 문자열로 전달받아, 이스케이프 처리를 위해 인코딩한다. 디코드는 그 반대<br><br>


## 암묵적 전역

```JavaScript
x // undefined
y // y is not defined

var x = 10;

function foo() {
   y = 20; 
}

foo();

console.log(x + y) // 30;
```

y는 키워드로 선언하지 않았기 때문에 `암묵적 전역`이 발생한다.<br>
따라서 y 는 `전역 객체의 프로퍼티`가 되고<br>
프로퍼티는 변수 `호이스팅이 발생하지 않는다.`<br>
전역변수 x와 전역객체의 프로퍼티 y를 더해 30이 도출된다.<br>











