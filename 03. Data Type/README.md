
# 데이터 타입

자바스크립트는 총 `7개의 타입`을 제공한다. 7개의 데이터 타입은 `원시타입`, `객체타입`으로 분류할 수 있다.

<hr>

## Type
1. Number
    - 숫자, 정수, 실수구분 없이 하나의 숫자타입만 존재
2. String
    - 문자열
3. Boolean
    - 논리적 참(true), 거짓(false)
4. undefined
    - var 키워드로 선언된 변수에 암묵적으로 할당되는 값
5. null
    - 값이 없다는 것을 의도적으로 명시할 때 사용되는 값
6. Symbol
    - ES6에서 추가된 7번째 타입
7. 객체타입
    - 객체, 함수, 배열 등등


## * 숫자타입
c나 자바와 달리 int, long, float등 다양한 숫자타입이 아닌 `Number`하나의 타입만을 제공한다.
```JavaScript
// 모두 정수 타입이다.
var integer = 10;
var double = 19.1;
var negative = -1;

var binary = 0b01000001; // 2진수
var octal = 00101; // 8잔수
var hex = 0*41; // 16잔수

// 표기법만 다를 뿐 모두 같은 값이다
console.log(binary); // 65
console.log(octal); // 65
console.log(hex); // 65
console.log(binary === octal); // true
console.log(octal === hex); // true
```

이는 정수로 표시된다 해도 사실은 실수라는 것을 의미하며 다음과 같은 상황이 발생할 수 있다.
```JavaScript
console.log(1 == 1.0); // true
```
<br>

> 숫자타입은 추가적으로 세 가지의 특별한 값도 표현할 수 있다. <br>
Infinity 양의 무한대<br>
-Infinity 음의 무한대<br>
NaN 산술연산불가<br>

<hr>

## * 문자열 타입

문자열타입은 텍스트데이터를 나타내는데 사용되며, 0개 이상의 16비트 유니코드 문자의 집합으로 전 세계 대부분의 문자를 표현할 수 있다.

문자열은 `작음따옴표`, `큰따옴표`, `백틱`으로 감싸 표현한다.
```JavaScript
var string;
string = '문자열’ ; //작은따옴표
string = "문자열";  //큰따옴표
string = '문자열';  //백틱
string = '작은따옴표로 감싼 문자열 내의"큰따옴표"는 문자열로 인식된다.’ ;
string = "큰따옴표로 감싼 문자열 내의'작은따옴표,는 문자열로 인식된다.";
```
> 문자열을 따옴표로 감싸는 이유는 식별자 혹은 토큰과 구별하기 위해서이다. 만약 따옴표로 감싸지 않는다면 JS엔진에서는 식별자로 인식한다.

> 템플릿 리터럴 <br>
ES6에서 도입되어 `멀티라인 문자열`과 `표현식 삽입`이 가능해졌다
```JavaScript
var template = '<ul>
<li><a href="#">Home</a></li>
</ul>';

var first = 'hello';
console.log(`${first} world`);
```

<hr>

## * 불리언 타입

Boolean타입은 참과 거짓을 나타낸다. true/false <br>
프로그램의 흐름을 제어하는 조건문에서 주로 사용된다.

<hr>

## * undefined 타입

undefined의 값은 undefined 가 유일하다.<br>
<br>
var 키워드로 선언한 변수는 `암묵적으로 undefined로 초기화`된다.<br>
그 이유는 var는 중복선언이 가능하다 따라서, 중복 변수 선언 시 기존 변수에 들어있던 쓰레기 값을 undefined로 초기화하기 위함이다.

<hr>

## * null 타입

null타입 또한 null이 유일하다.<br>
null은 `의도적`으로 개발자가 값이 비어있음 또는 더 이상 사용되지 않음을 나타내기 위해 할당한다.

<hr>

## * Symbol 타입

심벌타입은 ES6에서 추가된 7번째 타입으로 변경 불가능한 원시 값이다. 심벌 값은 다른 값과 중복되지 않는 `유일무이`한 값이다.<br>
따라서, 주로 충돌 위험이 없어야 할 때 주로 사용한다.

```JavaScript
//심벌 값 생성
var key = Symbol('key');
console.log(typeof key); // symbol

// 객체 생성
var obj = {};

// 이름이 충돌할 위험이 없는 유일무이한 값인 심벌을 프로퍼티 키로 사용한다.
obj[key] = 'value';
console.log(obj[key]); // value
```

<hr>

## * 객체 타입
위에서 설명했던 6가지의 원시타입과는 다른 객체타입으로, `변경 가능한 값`이며, JS를 이루고 있는 거의 모든 것이 객체라고 설명 가능하다.

<hr>

#### 데이터 타입의 필요성
1. 데이터 타입에 따라 효율적으로 메모리 공간을 할당한다.
2. 데이터 타입에 따라 메모리에 할당 된 데이터롤 효율적으로 읽어들일 수 있다.
3. 메모리에서 읽어들인 2진수를 어떻게 해석할지를 결정한다.

<hr>

## * 동적 타이핑

C나 자바같이 정적타입 언어는 변수를 선언 시 명시적으로 타입을 선언한 후 컴파일 과정에서 타입체크를 하여 일관성을 강제한다.<br>
그러나 JS는 선언이 아닌 `할당`에 의해 타입이 결정되고, 언제든 재할당이 `가능`하여 변수의 타입이 동적으로 계속 변화한다.<br>
이러한 특징을 `동적타이핑`이라 한다.<br>
<br>
장점 : 개발 시 굉장히 편리하다, 개발에 있어서 고민거리를 덜어내준다.<br>
단점 : 타입이 암묵적으로 변하므로 개발자는 확신할 수 없으므로 신뢰성이 떨어진다.<br>

<hr>

# * 변수 선언 시 주의점
1. 변수는 꼭 필요한 경우에 한해 제한적으로 사용한다. 암묵적 타입 변환에 의해 예기치 못한 오류가 발생할 수 있으니 무분별한 변수 남발은 주의햐아한다.
2. 변수의 스코프는 최대한 좁게 만들어야한다. 마찬가지로 필요한 경우가 아니라면 전역변수의 사용을 자제하는 것이 좋다.
3. 변수보다는 상수를 이용해 값의 변경을 억제한다.
4. 변수 이름은 데이터의 타입, 목적, 의미 등을 유추할 수 있도록 짓는 것이 효율적이다.


