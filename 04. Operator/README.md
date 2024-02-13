
# 연산자

`연산자`는 하나 이상의 표현식을 대상으로 `산술`, `할당`, `비교`, `논리`, `타입`, `지수연산`등을 수행해 하나의 `값`을 만든다.<br>
이 때 연산의 대상을 피연산자라고 한다. 피연산자는 값으로 평가될 수 있는 값이어야 한다.<br>
그리고, 피연산자와 연산자의 조합으로 이뤄진 연산자 표현식도 하나의 값으로 평가될 수 있는 값이어야 한다.<br>

ex)
```JavaScript
// 산술 연산자
5 * 4 // - 20

// 문자열 연결 연산자
'My name is ' + ' Lee' // — 'My name is Lee'

// 할당 연산자
color = 'red' // — 'red'

// 비교 연산자
3 > 5 // — false

// 논리 연산자
true && false // — false

// 타입 연산자
typeof 'Hi' // — string
```

<hr>

## 산술연산자

산술연산자는 피연산자를 대상으로 수학적인 연산을 수행해 새로운 값을 만든다. 연산이 불가능할 경우 `NaN`을 반환한다.

```JavaScript
// 이항 산술연산자
5 + 2
5 * 3

// 단항 산술연산자
var x = -1;
x++;
++x;

var y = "Hello";
+y // NaN

// 문자열 연결연산자
"1" + 2 // "12";
```

<hr>

## 암묵적 타입변환 또는 타입 강제변환
```JavaScript
1 + true // 2
1 + false // 1
"1" + 2 // "12"
```
위 예제에서 보다싶이 true, false는 1과 0으로 숫자 2는 문자열 2로 `암묵적인 타입변환`이 일어났다.

<hr>

## 할당 연산자
우항에 있는 피연산자의 평가결과를 좌항에 있는 변수에 할당한다. <br>
할당을 하기 때문에 기존 값이 변한다.
```JavaScript
var x;

x = 10;
console.log(x); // 10

x += 5; // x = x + 5;
console.log(x); // 15

x -= 5; // x = x - 5;
console.log(x); // 10

x *= 5; // x = x * 5;
console.log(x); // 50

x /= 5; // x = x / 5;
console.log(x); // 10

x %= 5; // x = x % 5;
console.log(x); // 0
```

<hr>

## 비교연산자 

좌항과 우항을 비교하여 값이 같은지 틀린지를 `Boolean값`으로 반환한다.
```JavaScript
var x = "5";
var y = 5;

x == y // true
x === y // false
x != y // false
x !== y // true

'0' == ''; // false
0 == ''; // true
0 == '0'; // true
false == 'fals은'; // false
false == '0'; // true
false == null; // false
false == undefined; // false
```

> NaN === NaN 은 유일하게 자신과 일치하지 않는 값이다.<br>
보다 정확한 예측 가능한 값의 비교를 원한다면 `Obejct.is` 메서드를 사용하자

<hr>

## 대소관계 비교 연산자
```JavaScript
var x = 1;
var y = 2;
x > y // false
x < y // true
x >= y // false
x <= y // true
```

<hr>

## 삼항조건 연산자
```JavaScript
조건식 ? 조건식이 참일 때 반환값 : 조건식이 거짓일 때 반환값;
```

if ... else 문을 사용하는 것을 삼항 조건 연산자를 통해 `유사하게 처리`할 수 있다.<br>
둘의 가장 큰 차이로는 `삼항조건 연산자는 값으로써 사용`할 수 있지만 `if...else는 조건[문]으로 값으로써 사용이 불가능`하다.<br>

<hr>

## 논리 연산자
```JavaScript
//논리합(//)연산자
true !! true; // true
true !! false; // true
false !! true; // true
false !! false; // false

// 논리곱(&&) 연산자
true && true; // true
true && false; // false
false && true; // false
false && false; // false

// 논리 부정(!) 연산자
!true; // false
!false; // true

// 논리 연산자를 이용한 암묵적인 타입변환
!0; // true
!"true" // false
```

<hr>

## 쉼표 연산자
쉼표 연산자는 피연산자부터 차례대로 평가하고, 마지막 피연산자의 평가가 끝나면 마지막 피연산자의 평가결과를 반환한다.

```JavaScript
var x, y, z;
x = 1, y = 2, z = 3; // 3
```

<hr>

## 그룹 연산자
소괄호()를 통해 감싸면 연산자의 `우선순위`에 영향을 줄 수 있다.

```JavaScript
10 * 2 + 3; // - 23

// 그룹 연산자를 사용하여 우선순위를 조절
10 * （2 + 3）； // - 50
```

<hr>

## typeof 연산자
피연산자의 데이터 타입을 문자열로 반환한다.

```JavaScript
typeof '' // “string"
typeof 1 // "number"
typeof NaN // "number”
typeof true // "boolean"
typeof undefined // "undefined"
typeof Symbol() // "symbol"
typeof null // "object"
typeof [] // "object"
typeof {} // "object"
typeof new Date() // "object”
typeof /test/gi // "object"
typeof function () {} // "function”
```

<hr>

## 지수 연산자
ES7에서 도입이 되었으며, `거듭제곱`하여 숫자 값을 반환한다.

```JavaScript
2 ** 2; // 4
2 ** 0; // 1
2 ** -2; // 0.25

// 음수를 거듭제곱하려면 소괄호로 감싸야 한다.
(-5) ** 2; // 25

// Math.pow 메서드와 동일하게 동작한다.
```

<hr>

## 그 외의 연산자

`?.` -> 옵셔널 체이닝<br>
`??` -> null 병합 연산자<br>
`delete` -> 프로퍼티 삭제<br>
`new` -> 생성자 함수 호출<br>
`instanceof` -> 좌변의 객체가 우변의 생성자 함수와 연결된 인스턴스인지 판별<br>
`in` -> 프로퍼티 존재 확인<br>
