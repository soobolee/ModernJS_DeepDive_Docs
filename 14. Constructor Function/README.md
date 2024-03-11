
# Object 생성자 함수

<br>

생성자 함수 = new 연산자와 함께 호출하여 `객체를 생성`하는 함수<br>
인스턴스 = 생성자 함수에 의해 `생성된 객체`<br>
<br>

Object뿐만 아니라 `String`, `Number`, `Boolean`, `Function`, `Array`, `Date`, `RegExp`, `Promise` 등의 생성자함수 제공<br>
<br>

1. 객체 리터럴에 의한 객체 생성 방식의 문제점<br>

- 매우 간단하게 객체 생성 가능하지만 여러 개 생성시 매번 같은 코드를 기술해야한다.<br>

<br>

> 객체는 고유의 상태를 표현한다. 그리고 메서드를 통해 상태 데이터인 프로파티를 참조하고 조작하는 동작을 표현한다. 따라서, 프로퍼티 값이 달라도 메서드는 동일한 내용이 많다.

<hr>

2. 생성자 함수에 의한 객체 생성 방식의 장점<br>

- 생성자 함수를 사용하여 구조가 동일한 객체 `여러 개를` `간편`하게 생성할 수 있다.<br>

<br>

```JavaScript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자와 함께 객체(인스턴스) 생성
const circle1 = new Circle(1);
const circle2 = new Circle(5);

// 각 Person 객체의 메서드 호출
console.log(circle1.getDiameter()); // 2
console.log(circle2.getDiameter()); // 10

const circle3 - Circle(3);
// new 연산자 없이 호출하면 일반 함수로써 기능한다. return값은 없으므로 암묵적으로 undefined가 반환된다.
```

<br>

위 예제처럼 동일한 메서드에 프로퍼티만 달리해서 여러 객체를 간편하게 생성이 가능하다.<br>
* new 연산자는 필수이다.<br>

<br>

## 생성자 함수의 인스터스 생성 과정

<br>

1. 인스턴스 생성과 this바인딩<br>
   암묵적으로 `빈 객체`가 생성되고, 생성된 객체[인스턴스]는 `this에 바인딩`된다. 이러한 처리는 런타임 이전 `평가과정에서 실행`된다.<br>

<br>

2. 인스턴스 초기화<br>
   this에 바인딩되어 있는 `변수를 할당`한다. 고정값이나 매개변수로 받은 값을 개발자가 직접 처리한다.<br>
<br>

3. 인스턴스 반환<br>
   위 작업이 완료되면 완성된 객체의 `this`를 암묵적으로 `반환`한다.<br>
   명시적으로 객체를 반환하면 해당 객체가 반환되지만 `원시값 반환 시 해당 this객체가 반환`된다.<br>

<br>

## 생성자 함수와 생성자가 아닌 함수 구분

<br>

생성자 함수로 호출한다는 것은 new연산자와 함께 호출해서 객체를 생성한다는 것이다.<br>

<br>

`constructor` = 생성자 함수로 호출할 수 있는 형태 [함수선언문], [함수표현식]<br>
`non-constructor` = 생성자 함수로 호출할 수 없는 형태 [화살표함수], [메서드]<br>

<br>

모든 함수객체는 반드시 내부 메서드 `[[Call]]` 을 가지고 있어`[Callable]` 함수 호출 시 호출된다.<br>
모든 함수 객체가 `[[Construct]]`를 가지는 것이다 가진 함수는 생성자 함수로써 호출할 수 있다.<br>


<br>

## new 연산자

<br>

new 연산자를 사용해 [[Construct]]를 가진 함수를 호출하면 [[Construct]]가 호출되어 `생성자 함수`로써 동작한다.<br>
반대로 new 없이 함수를 호출하면 [[Call]]이 호출되어 `일반함수`로써 호출된다.<br>
